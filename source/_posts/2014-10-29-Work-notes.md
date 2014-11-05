title: 2014-10-29-工作笔记
date: 2014-10-29 10:50:13
tags: work
---

#### express中设置 *app.set('json spaces', 40)* 以格式化的形式输出JSON文件

### android 硬件加速后webview闪烁问题

android webview 启用硬件加速后(开启硬件加速是在manifest中加入： android:hardwareAccelerated="true")
但是开启硬件加速后在较老的硬件设备中比如小米2 webview有可能会出现闪烁的问题，解决方法是在webview中设置：

        setLayerType(View.LAYER_TYPE_SOFTWARE, null);


这是把webview 中的硬件加速关闭。设置LAYER_TYPE_SOFTWARE 后会把当前view转为bitmap保存。这样就不能开多个webview， 否则会报out of memory。


解决方法是在webview中加入：

	    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
	        invalidate();
	            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
	    }

		setBackgroundColor(Color.parseColor("#000000")); //ok 不会闪黑屏

### android 4.4 webview 的 viewPort设置无效

android 4.4 对应android SDK 为19 以后 webview基于 Chromium，与PC的Chrome内核一致，性能上有了极大的提升,但是默认情况下 meta标签中的viewport设置无效。需要添加如下代码设置


	    WebSettings settings = webView.getSettings();
	    settings.setLoadWithOverviewMode(true);
	    settings.setUseWideViewPort(true);

### javascript instanceof

### 关于 javascript  instanceof
之前用这个关键字来判断 一个对象是否是一个构造函数的实例,比如 

    var a=[1,23,4];
    console.log( a instanceof Array);//true


   对于 a instanceof B  a 为一个对象，B一个（构造）函数其本质是是判断 B的原型（B.prototype)是否在 a的原型链上。



		function A() {
	        this.name1 = 'ee';
	        console.log("构造A的函数");
	    }
	    A.prototype = {
	        constructor: A,
	        age: 30
	    };
	    function B() {
	        this.profile = '22';
	    }
	    B.prototype= A.prototype;
	    var b = new B();
	    console.log(b instanceof A);
	    function C() {

	    }
	    C.prototype= A.prototype;     //把A的原型放在 C的原型链上
	    console.log(b instanceof C ); //显然 b不是C的实例，但是输入为true,
	                                 // 因为b的原型==A的原型,所以C的原型在b的原型链上

另外 对于以上 *a instanceof Array* 前提是必须是在同一个窗体下才能返回为true，如果 a是在其他Iframe的下创建的就不行了，每个窗体都是独立的环境。窗体frame1中的Array 与frame2中的Array不是同一个东西的。


