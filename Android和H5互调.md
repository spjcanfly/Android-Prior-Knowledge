#安卓和H5交互，实质上就是java和javascript的交互

首先在main的文件下建立一个assets文件夹，里面存在网页的代码文件

下面是一个javascript调java的一个小demo

     import android.app.Activity;
     import android.os.Bundle;
     import android.text.TextUtils;
     import android.view.View;
     import android.webkit.JavascriptInterface;
     import android.webkit.WebSettings;
     import android.webkit.WebView;
     import android.webkit.WebViewClient;
     import android.widget.Button;
     import android.widget.EditText;
     import android.widget.Toast;


    public class JavaAndJSActivity extends Activity implements View.OnClickListener {
    private EditText etNumber;
    private EditText etPassword;
    private Button btnLogin;
    /**
     * 加载网页或者说H5页面
     */
    private WebView webView;
    private WebSettings webSettings;

    /**
     * 
     * 这些是查找控件
     */
    private void findViews() {
        setContentView(R.layout.activity_java_and_js);
        etNumber = (EditText) findViewById(R.id.et_number);
        etPassword = (EditText) findViewById(R.id.et_password);
        btnLogin = (Button) findViewById(R.id.btn_login);
        btnLogin.setOnClickListener(this);

    }

    /**
     *登陆按钮的点击事件
     */
    @Override
    public void onClick(View v) {
        if (v == btnLogin) {
            // Handle clicks for btnLogin
            login();
        }
    }

    private void login() {
        String numebr = etNumber.getText().toString().trim();//账号
        String password = etPassword.getText().toString().trim();//密码
        if (TextUtils.isEmpty(numebr) || TextUtils.isEmpty(password)) {
            Toast.makeText(JavaAndJSActivity.this, "账号或者密码为空", Toast.LENGTH_SHORT).show();
        } else {
            //把账号传递给html页面
            //登录
            login(numebr);
        }
    }

    /**
     * 传入账号，加载网页
     * @param numebr
     */
    private void login(String numebr) {
        webView.loadUrl("javascript:javaCallJs("+"'"+numebr+"'"+")");
        setContentView(webView);
    }


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        findViews();
        initWebView();
    }

    private void initWebView() {
        webView = new WebView(this);
        //设置支持javaScript
        webSettings = webView.getSettings();
        //设置支持javaScript
        webSettings.setJavaScriptEnabled(true);
        //设置双击变大变小
        webSettings.setUseWideViewPort(true);
        //增加缩放按钮
        webSettings.setBuiltInZoomControls(true);
        //设置文字大小
        webSettings.setTextZoom(100);
        //不让从当前网页跳转到系统的浏览器中
        webView.setWebViewClient(new WebViewClient() {
            //当加载页面完成的时候回调
            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
            }
        });

        //添加javaScript接口
        webView.addJavascriptInterface(new MyJavascriptInterface(),"android");

        //可以加载网络的页面，也可以加载应用内置的页面
        webView.loadUrl("file:///android_asset/JavaAndJavaScriptCall.html");
        webView.loadUrl("http://192.168.21.165:8080/JavaAndJavaScriptCall.html");
    }


    private class MyJavascriptInterface {
        @JavascriptInterface
        public void showToast(){
            Toast.makeText(JavaAndJSActivity.this, "我被js调用了", Toast.LENGTH_SHORT).show();
          }
      
        }
     }

如果build.gradle中的targetSdkVersion的版本低的话，比如16，那么
不用添加 @JavascriptInterface 的注解就可以实现javascript调java，但是如果是高版本，比如说23，addJavascriptInterface这个方法就过时了，必须添加注解才行。

    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
    <script type="text/javascript">

	function javaCallJs(){
		 document.getElementById("content").innerHTML +=   
	         "<br\>java调用了js无参函数";
	}
	
	function javaCallJs(arg){
		 document.getElementById("content").innerHTML =
	         ("欢迎："+arg );
	}
	
	
    function showDialog(){
      alert("杰粉们你好,我是来自javascript");
    }
   
    </script>

    </head>

    <body>

    <div align="left" id="content"> 杰粉</div>
    <div align="right">光临杰哥</div>

    <p><img src="http://jiege.com/images/logo.gif"></p>

    <input type="button" value="点击Android被调用" onclick="window.android.showToast()" />

    </body>

    </html>

上面 onclik中 android 字段， 对应于的

     //添加javaScript接口
     webView.addJavascriptInterface(new MyJavascriptInterface(),"android");

而 showToast方法 对应于下面的方法名，一定要一致才行。

     private class MyJavascriptInterface {
        @JavascriptInterface
        public void showToast(){
            Toast.makeText(JavaAndJSActivity.this, "我被js调用了", Toast.LENGTH_SHORT).show();
          }     
        }