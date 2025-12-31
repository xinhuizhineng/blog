---
title: "Angular + Ng-Zorro 模态框拖拽功能"
author: "常香玉"              # 文章作者
description: "Angular + Ng-Zorro 模态框拖拽功能"
date: 2024-01-22T10:05:18+08:00
lastmod: 2024-01-22         # 文章修改日期
categories: [
   "Web前端"
]
tags: [
   "Web前端",
   "Angular"
]
---

## 基本原理
通过监听鼠标的按下（mousedown），弹起（mouseup），移动（mousemove）三个事件来计算移动的距离，从而调整模态框的新位置。
- 鼠标按下（mousedown）：
  - 记录鼠标按下时的位置和弹窗当前的位置，并设置弹窗左侧和顶部边距为当前位置；
  - 将 `canMove` 标志设置为 `true`，表示模态框可以移动。
- 鼠标弹起（mouseup）：
  - 将 `canMove` 标志设置为 `false`，表示停止移动模态框。
- 鼠标移动（mousemove）：
  - 如果 `canMove` 为 `true`，则计算鼠标移动距离。
  - 根据鼠标的移动距离调整模态框的新位置，使用渲染器设置模态框的新左侧和顶部偏移值，从而实现拖拽效果。
## 通过nz-modal模板生成的弹窗
实现步骤：
### 1.通过指令来绑定拖拽方法( drag-modal.directive.ts )：
```
import { Directive, Input } from '@angular/core';
import { NzModalRef } from 'ng-zorro-antd/modal';
import { ModalDragService } from './drag-modal.service.ts';
@Directive({
    selector: '[dragModal]'
})
export class DragModalDirective {
    @Input() dragModal: NzModalRef;

    constructor(private modalDragService: ModalDragService) {

    }

    ngAfterViewInit() {
        setTimeout(() => {
            this.modalDragService.enableModalDrag(this.dragModal);
        }, 1000);
    }
} 
```
### 2.注册模块 (modal-drag.module.ts)
```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { DragModalDirective } from "./drag-modal.directive";
import { ModalDragService } from './drag-modal.service.ts';

@NgModule({
    providers: [ModalDragService],
    declarations: [DragModalDirective],
    imports: [
        CommonModule,
    ],
    exports: [DragModalDirective]
})
export class ModalDragModule { } 
```
使用方法：
  1. 将 `ModalDragModule` 导入到需要使用弹窗拖拽的功能模块中。
  2. 在 `<nz-modal>` 标签中增加一个模板引用变量（例如 `#modal`）。
  3. 将该变量传递给 `dragModal` 指令。
图1
图2

## 通过NzModalService服务创建的弹窗
实现步骤：
利用服务把事件绑定在modal的header上，通过控制mousedown,mouseup,mousemove，来实现拖拽
```
import { Injectable, RendererFactory2 } from '@angular/core';
@Injectable({
    providedIn: 'root'
})
export class ModalDragService {
    constructor(private rendererFactory2: RendererFactory2,) { }

    // 激活弹窗拖拽
    enableModalDrag(refModal) {
        refModal.afterOpen.subscribe(() => {
            const render = this.rendererFactory2.createRenderer(null, null);
            // 整个悬浮层的dom
            const modalBackground = refModal.getElement();
            // 弹窗的dom
            const modalElement = refModal.getElement().querySelector('.ant-modal-content');
            // 弹窗header的dom
            const modalTitleElement = this.createModalTitleElement(render, modalElement);
            // 绑定拖拽事件
            this.dragListen(render, modalTitleElement, modalElement, modalBackground);
        })
    }

    // 自定义弹窗header拖拽区域的大小
    createModalTitleElement(render, modalElement) {
        let element = document.createElement('div') as any;
        render.setStyle(element, 'width', '100%');
        render.setStyle(element, 'height', '54px');
        render.setStyle(element, 'position', 'absolute');
        render.setStyle(element, 'top', '0');
        render.setStyle(element, 'left', '0');
        render.setStyle(element, 'cursor', 'move');
        render.setStyle(element, '-moz-user-select', 'none');
        render.setStyle(element, '-webkit-user-select', 'none');
        render.setStyle(element, '-ms-user-select', 'none');
        render.setStyle(element, '-khtml-user-select', 'none');
        render.setStyle(element, 'user-select', 'none');
        render.appendChild(modalElement, element);
        return element;
    }

    // 监听鼠标的mousedown，mouseup，mousemove事件
    dragListen(render, modalTitleElement, modalElement, modalBackground) {
        render.listen(modalTitleElement, 'mousedown', function (event) {
            // 记录了鼠标按下时的位置（mouseDownX 和 mouseDownY）。
            // 获取了模态框当前的位置（modalX 和 modalY）。
            // 设置模态框的左侧和顶部边距为当前位置。
            // 将 canMove 标志设置为 true，表示模态框可以移动。
            this.mouseDownX = event.clientX;
            this.mouseDownY = event.clientY;
            this.modalX = modalElement.offsetLeft;
            this.modalY = modalElement.offsetTop;
            render.setStyle(modalElement, 'left', `${this.modalX}px`);
            render.setStyle(modalElement, 'top', `${this.modalY}px`);
            this.canMove = true;
        }.bind(this));
        render.listen(modalTitleElement, 'mouseup', function (event) {
            // 将 canMove 标志设置为 false，表示停止移动模态框
            this.canMove = false;
        }.bind(this));
        render.listen(modalBackground, 'mousemove', function (event) {
            // 如果 canMove 为 true，则计算鼠标移动距离。
            // 根据鼠标的移动距离调整模态框的新位置。
            // 使用渲染器设置模态框的新左侧和顶部偏移值，从而实现拖拽效果。
            if (this.canMove) {
                let moveX = event.clientX - this.mouseDownX;
                let moveY = event.clientY - this.mouseDownY;
                let newModalX = this.modalX + moveX;
                let newModalY = this.modalY + moveY;
                render.setStyle(modalElement, 'left', `${newModalX}px`);
                render.setStyle(modalElement, 'top', `${newModalY}px`);
            }
        }.bind(this));
    }
} 
```
在初始化`modal`时，使用`modalDragService`的`enableModalDrag`方法激活拖拽功能；
图3
使用方法：
图4


