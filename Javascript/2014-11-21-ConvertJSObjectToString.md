---
layout: post
title: Convert Javascript object To String
tags : [JSON, Javascript]
category : Javascript
---


    function objToString (obj) {
        var str = '';
        for (var p in obj) {
            if (obj.hasOwnProperty(p)) {
                str += p + '::' + obj[p] + '\n';
            }
        }
        return str;
    }


or 

    JSON.stringify(obj);

> Written with [StackEdit](https://stackedit.io/).