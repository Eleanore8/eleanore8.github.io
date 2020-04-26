---
layout: default
title:  "MutationObserver"
date:   2020-03-11 17:50:00
des:  ""
motto:  ""
categories: js
---

## MutationObserver 构造函数

Mutation Observer API 用来监视 DOM 变动。
DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动。

* 等待所有脚本任务完成后，才会运行（即异步触发方式）

* 把 DOM 变动记录封装成一个数组进行处理，而不是一条条个别处理 DOM 变动

* 既可以观察 DOM 的所有类型变动，也可以指定只观察某一类变动


    const observe = new MutationObserver((mutations, observer) => {
        mutations.forEach((mutation) => {
            console.log(mutation); // MutationRecord 对象
        })
    }));

新建一个观察器实例，同时指定这个实例的回调函数。

## 实例方法

### observe() 

参数：

* 所要观察的 DOM 节点

* 配置对象，指定所要观察的特定变动

启动监听

    const divDom = document.getElementById('mutationObserve');
    const config = {
        childList: true, // 子节点的变动
        attributes: true // 属性的变动
        characterData: true, // 节点内容或节点文本的变动
        subtree: true, // 是否将该观察器应用于该节点的所有后代节点
        attributeOldValue: true, // 观察attributes变动时，是否需要记录变动前的属性值
        characterDataOldValue: true // 观察characterData变动时，是否需要记录变动前的值
        attributeFilter: ['class','src'] // 需要观察的特定属性
    };
    observer.observe(divDom, config);
    
### disconnect()

停止观察，DOM 再发生变动，也不会触发观察器。

    observer.disconnect();
    
### takeRecords()

清除变动记录。即不再处理未处理的变动。该方法返回变动记录的数组。

## MutationRecord 对象

* type: 观察的变动类型（attributes、characterData或者childList）

* target: 发生变动的dom节点

* addedNodes: 新增的DOM节点

* removedNodes：删除的DOM节点

* previousSibling：前一个同级节点，如果没有则返回null

* nextSibling：下一个同级节点，如果没有则返回null

* attributeName：发生变动的属性。如果设置了attributeFilter，则只返回预先指定的属性

* oldValue：变动前的值。
这个属性只对attribute和characterData变动有效，如果发生childList变动，则返回null

## 使用MutationObserver对象封装一个监听 DOM 生成的函数。

    (function (win) {
        const listeners = [];
        const doc = win.document;
        const MutationObserver = win.MutationObserver || win.webKitMutationObserver;
        let observer;

        function ready(selector, fn) {
            // 储存选择器和回调函数
            listeners.push({
                selector,
                fn
            });
            if (!observer) {
                // 监听DOM变化
                observer = new MutationObserver(check);
                observer.observe(doc.documentElement, {
                    childList: true,
                    subtree: true
                });
            }
            // 检查该节点是否已经在DOM中
            check();
        }

        function check() {
            // 检查是否匹配已储存的节点
            listeners.forEach((listener) => {
                // 检查指定节点是否有匹配
                const elements = doc.querySelectorAll(listener.selector);
                elements.forEach((element) => {
                    if (!element.ready) {
                        element.ready = true;
                        // 对该节点调用回调函数
                        listener.fn.call(element, element);
                    }
                });
            });
        }

        // 对外暴露ready
        win.ready = ready;
    })(this);

    // 使用方法
    ready('#mutationObserve', (element) => {});
