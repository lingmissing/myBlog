---
title: jquery小技巧
date: 2016-04-06 16:47:55
tags: jquery
categories: 代码如诗，前端如画
---

#### 禁止右键点击 ####

```javascript
$(document).ready(function(){
    $(document).bind("contextmenu",function(e){
        return false;
    });
});
```
#### 隐藏搜索文本框文字 ####
```javascript
$(document).ready(function() {
$("input.text1").val("Enter your search text here");
   textFill($('input.text1'));
});
function textFill(input){ //input focus text function
  var originalvalue = input.val();
  input.focus( function(){
    if( $.trim(input.val()) == originalvalue ){ input.val(''); }
  });
  input.blur( function(){
    if( $.trim(input.val()) == '' ){ input.val(originalvalue); }
  });
}
```
<!--more-->

#### 在新窗口中打开链接 ####
```javascript
$(document).ready(function() {
   //案例 1: 链接以新开页的形式打开
   $('a[href^="http://"]').attr("target", "_blank");

   //案例 2: 只有rel="external" 属性的链接将在新窗口打开
   //<a href="http://www.opensourcehunter.com" rel=external>open link</a>
   $('a[@rel$='external']').click(function(){
      this.target = "_blank";
   });});
```
#### 检测浏览器 ####
注: 在版本jQuery 1.4中，$.support 替换掉了$.browser 变量
```javascript
$(document).ready(function() {
// Target Firefox 2 and aboveif ($.browser.mozilla && $.browser.version >= "1.8" ){
    // do something}

// Target Safariif( $.browser.safari ){
    // do something}

// Target Chromeif( $.browser.chrome){
    // do something}

// Target Caminoif( $.browser.camino){
    // do something}

// Target Operaif( $.browser.opera){
    // do something}

// Target IE6 and belowif ($.browser.msie && $.browser.version <= 6 ){
    // do something}

// Target anything above IE6if ($.browser.msie && $.browser.version > 6){
    // do something}});
```
#### 预加载图片 ####
这段代码将会阻止加载所有图片，这对于一个网站有许多的图片将变得很实用。
```javascript
$(document).ready(function() {
  jQuery.preloadImages = function(){
    for(var i = 0; i<ARGUMENTS.LENGTH; jQuery(?<img { i++)>").attr("src", arguments[i]);
  }
  // 如何使用
  $.preloadImages("image1.jpg");
});
```
#### 页面样式切换 ####
```javascript
$(document).ready(function() {
    $("a.Styleswitcher").click(function() {
        //通过rel属性来链接样式
        $('link[rel=stylesheet]').attr('href' , $(this).attr('rel'));
    });
});
```
#### 列高度相同 ####
如果使用了两个CSS列，使用此种方式可以是两列的高度相同。
```javascript
$(document).ready(function() {
  function equalHeight(group) {
    tallest = 0;
    group.each(function() {
      thisHeight = $(this).height();
      if(thisHeight > tallest) {
        tallest = thisHeight;
      }
    });
    group.height(tallest);
  }
  $(document).ready(function() {
    equalHeight($(".left"));
    equalHeight($(".right"));
  });
});
```
#### 动态控制页面字体大小 ####
用户可以改变页面字体大小
```javascript
$(document).ready(function() {
  // 重置字体大小
  var originalFontSize = $('html').css('font-size');
  $(".resetFont").click(function(){
    $('html').css('font-size', originalFontSize);
  });
  // Increase the font size(bigger font）
  $(".increaseFont").click(function(){
    var currentFontSize = $('html').css('font-size');
    var currentFontSizeNum = parseFloat(currentFontSize, 10);
    var newFontSize = currentFontSizeNum*1.2;
    $('html').css('font-size', newFontSize);
    return false;
  });
  // Decrease the font size(smaller font)
  $(".decreaseFont").click(function(){
    var currentFontSize = $('html').css('font-size');
    var currentFontSizeNum = parseFloat(currentFontSize, 10);
    var newFontSize = currentFontSizeNum*0.8;
    $('html').css('font-size', newFontSize);
    return false;
  });
});
```
#### 返回页面顶部功能 ####
```javascript
$(document).ready(function() {
$('a[href*=#]').click(function() {
 if (location.pathname.replace(/^\//,'') == this.pathname.replace(/^\//,'')
 && location.hostname == this.hostname) {
   var $target = $(this.hash);
   $target = $target.length && $target
   || $('[name=' + this.hash.slice(1) +']');
   if ($target.length) {
  var targetOffset = $target.offset().top;
  $('html,body')
  .animate({scrollTop: targetOffset}, 900);
    return false;
   }
  }
  });
// how to use
// place this where you want to scroll to<A name=top></A>// the link<A href="#top">go to top</A>
});
```
#### 获得鼠标指针ＸＹ值 ####
Want to know where your mouse cursor is?
```javascript
$(document).ready(function() {
   $().mousemove(function(e){
     //display the x and y axis values inside the div with the id XY
    $('#XY').html("X Axis : " + e.pageX + " | Y Axis " + e.pageY);
  });// how to use<DIV id=XY></DIV>
});
```
#### 返回顶部按钮 ####
你可以利用 animate 和 scrollTop 来实现返回顶部的动画，而不需要使用其他插件。
```javascript
// Back to top
$('a.top').click(function () {
  $(document.body).animate({scrollTop: 0}, 800);
  return false;});
<!-- Create an anchor tag --><a class="top" href="#">Back to top</a>
```
改变 scrollTop 的值可以调整返回距离顶部的距离，而 animate 的第二个参数是执行返回动作需要的时间(单位：毫秒)。
#### 预加载图片 ####
如果你的页面中使用了很多不可见的图片（如：hover 显示），你可能需要预加载它们：
```javascript
$.preloadImages = function () {
  for (var i = 0; i < arguments.length; i++) {
    $('<img>').attr('src', arguments[i]);
  }};

$.preloadImages('img/hover1.png', 'img/hover2.png');
```
#### 检查图片是否加载完成 ####
有时候你需要确保图片完成加载完成以便执行后面的操作：
```javascript
$('img').load(function () {
  console.log('image load successful');
});
```
你可以把 img 替换为其他的 ID 或者 class 来检查指定图片是否加载完成。
#### 自动修改破损图像 ####
如果你碰巧在你的网站上发现了破碎的图像链接，你可以用一个不易被替换的图像来代替它们。添加这个简单的代码可以节省很多麻烦：
```javascript
$('img').on('error', function () {
  $(this).prop('src', 'img/broken.png');
});
```
即使你的网站没有破碎的图像链接，添加这段代码也没有任何害处。
#### 鼠标悬停(hover)切换 class 属性 ####
假如当用户鼠标悬停在一个可点击的元素上时，你希望改变其效果，下面这段代码可以在其悬停在元素上时添加 class 属性，当用户鼠标离开时，则自动取消该 class 属性：
```javascript
$('.btn').hover(function () {
  $(this).addClass('hover');
  }, function () {
    $(this).removeClass('hover');
  });
```
你只需要添加必要的CSS代码即可。如果你想要更简洁的代码，可以使用 toggleClass 方法：
```javascript
$('.btn').hover(function () { 
  $(this).toggleClass('hover'); 
});
```
注：直接使用CSS实现该效果可能是更好的解决方案，但你仍然有必要知道该方法。
#### 禁用 input 字段 ####
有时你可能需要禁用表单的 submit 按钮或者某个 input 字段，直到用户执行了某些操作（例如，检查“已阅读条款”复选框）。可以添加 disabled 属性，直到你想启用它时：
```javascript
$('input[type="submit"]').prop('disabled', true);
```
你要做的就是执行 removeAttr 方法，并把要移除的属性作为参数传入：
```javascript
$('input[type="submit"]').removeAttr('disabled');
```
#### 阻止链接加载 ####
有时你不希望链接到某个页面或者重新加载它，你可能希望它来做一些其他事情或者触发一些其他脚本，你可以这么做：
```javascript
$('a.no-link').click(function (e) {
  e.preventDefault();
});
```
#### 切换 fade/slide ####
fade 和 slide 是我们在 jQuery 中经常使用的动画效果，它们可以使元素显示效果更好。但是如果你希望元素显示时使用第一种效果，而消失时使用第二种效果，则可以这么做：
```javascript
// Fade
$('.btn').click(function () {
  $('.element').fadeToggle('slow');
});
// Toggle
$('.btn').click(function () {
  $('.element').slideToggle('slow');
});
```
#### 简单的手风琴效果 ####
这是一个实现手风琴效果快速简单的方法：
```javascript
// Close all panels
$('#accordion').find('.content').hide();
// Accordion
$('#accordion').find('.accordion-header').click(function () {
  var next = $(this).next();
  next.slideToggle('fast');
  $('.content').not(next).slideUp('fast');
  return false;
});
```
#### 让两个 DIV 高度相同 ####
有时你需要让两个 div 高度相同，而不管它们里面的内容多少。可以使用下面的代码片段：
```javascript
var $columns = $('.column');
var height = 0;
$columns.each(function () {
  if ($(this).height() > height) {
    height = $(this).height();
  }
});
$columns.height(height);
```
这段代码会循环一组元素，并设置它们的高度为元素中的最大高。
#### 验证元素是否为空 ####
This will allow you to check if an element is empty.
```javascript
$(document).ready(function() {
  if ($('#id').html()) {
   // do something
   }
});
```
#### 替换元素 ####
```javascript
$(document).ready(function() {
   $('#id').replaceWith('<DIV>I have been replaced</DIV>');
});
```
#### jQuery延时加载功能 ####
```javascript
$(document).ready(function() {
   window.setTimeout(function() {
     // do something
   }, 1000);
});
```
#### 移除单词功能 ####
```javascript
$(document).ready(function() {
   var el = $('#id');
   el.html(el.html().replace(/word/ig, ""));
});
```
#### 验证元素是否存在于jquery对象集合中 ####
```javascript
$(document).ready(function() {
   if ($('#id').length) {
  // do something
  }
});
```
#### 使整个DIV可点击 ####
```javascript
$(document).ready(function() {
    $("div").click(function(){
      //get the url from href attribute and launch the url
      window.location=$(this).find("a").attr("href"); return false;
    });// how to use<DIV><A href="index.html">home</A></DIV>

});
```
#### ID与Class之间转换 ####
当改变Window大小时，在ID与Class之间切换
```javascript
$(document).ready(function() {
   function checkWindowSize() {
    if ( $(window).width() > 1200 ) {
        $('body').addClass('large');
    }
    else {
        $('body').removeClass('large');
    }
   }
  $(window).resize(checkWindowSize);
});
```
#### 克隆对象 ####
```javascript
$(document).ready(function() {
   var cloned = $('#id').clone();// how to use<DIV id=id></DIV>

});
```
#### 使元素居屏幕中间位置 ####
```javascript
$(document).ready(function() {
  jQuery.fn.center = function () {
      this.css("position","absolute");
      this.css("top", ( $(window).height() - this.height() ) / 
      2+$(window).scrollTop() + "px");
      this.css("left", ( $(window).width() - this.width() ) / 
      2+$(window).scrollLeft() + "px");
      return this;
  }
  $("#id").center();
});
```
#### 写自己的选择器 ####
```javascript
$(document).ready(function() {
   $.extend($.expr[':'], {
       moreThen1000px: function(a) {
           return $(a).width() > 1000;
      }
   });
  $('.box:moreThen1000px').click(function() {
      // 创建一个简单的js弹窗
      alert('The element that you have clicked is over 1000 pixels wide');
  });
});
```
#### 统计元素个数 ####
```javascript
$(document).ready(function() {
   $("p").size();
});
```
#### 使用自己的 Bullets ####
使用自己的bullets而不是标准bullets?
```javascript
$(document).ready(function() {
   $("ul").addClass("Replaced");
   $("ul > li").prepend("‒ ");
 // how to use
 ul.Replaced { list-style : none; }
});
```
#### 禁用Jquery（动画）效果 ####
```javascript
$(document).ready(function() {
    jQuery.fx.off = true;
});
```
#### 与其他Javascript类库冲突解决方案 ####
```javascript
$(document).ready(function() {
   var $jq = jQuery.noConflict();
   $jq('#id').show();
});
```

> 原文地址：[人人都会的35个Jquery小技巧](http://segmentfault.com/a/1190000003902322)