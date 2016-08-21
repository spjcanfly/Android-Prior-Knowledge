##1.Http协议：

请求包结构：请求行，请求头部，请求体

    POST /meme.php/home/user/login HTTP/1.1（请求行）
    Host: 114.215.86.90
    Cache-Control: no-cache
    Postman-Token: bd243d6b-da03-902f-0a2c-8e9377f6f6ed
    Content-Type: application/x-www-form-urlencoded（请求头）

    tel=13637829200&password=123456

需要看懂上面的请求的格式有：

请求行：请求方式  请求的地址  协议的版本号

发送请求后就会有响应的报文

响应报文结构：状态行，响应头，响应体

    HTTP/1.1 200 OK   （状态行）
    Date: Sat, 02 Jan 2016 13:20:55 GMT
    Server: Apache/2.4.6 (CentOS) PHP/5.6.14
    X-Powered-By: PHP/5.6.14
    Content-Length: 78
    Keep-Alive: timeout=5, max=100
    Connection: Keep-Alive
    Content-Type: application/json; charset=utf-8（响应头）

    {"status":202,"info":"\u6b64\u7528\u6237\u4e0d\u5b58\u5728\uff01","data":null}

状态行：协议版本，状态码，状态码描述。

Get方式：  在url中填写参数（所以get的参数放在请求行中）：

      http://xxxx.xx.com/xx.php?params1=value1&params2=value2
切记：get方式有中文需要编码：URLEncoder.encode(params, "gbk");

Post方式（键值对）： 参数是经过编码放在（请求体）中的。编码包括x-www-form-urlencoded 与 form-data。

所以get方式和post方式的本质的区别就是：get的参数放在请求行中，post的参数放在请求体中的。

x-www-form-urlencoded的编码方式是这样：
       tel=13637829200&password=123456

form-data的编码方式是这样：

     ----WebKitFormBoundary7MA4YWxkTrZu0gW
     Content-Disposition: form-data; name="tel"

     13637829200
     ----WebKitFormBoundary7MA4YWxkTrZu0gW
     Content-Disposition: form-data; name="password"

     123456
     ----WebKitFormBoundary7MA4YWxkTrZu0gW


##2.Android访问网络使用哪种方式好？

在Android 2.2版本之前，HttpClient拥有较少的bug，因此使用它是最好的选择。API数量过多，很难在不破坏兼容性的情况下对它进行升级和扩展。

而在Android 2.3版本及以后，HttpURLConnection则是最佳的选择。它的API简单，体积较小，因而非常适用于Android项目。压缩和缓存机制可以有效地减少网络访问的流量，在提升速度和省电方面也起到了较大的作用。对于新的应用程序应该更加偏向于使用。

下面就是HttpURLConnection的使用：

    public class NetUtils {
        public static String post(String url, String content) {
            HttpURLConnection conn = null;
            try {
                // 创建一个URL对象
                URL mURL = new URL(url);
                // 调用URL的openConnection()方法,获取HttpURLConnection对象
                conn = (HttpURLConnection) mURL.openConnection();

                conn.setRequestMethod("POST");// 设置请求方法为post
                conn.setReadTimeout(5000);// 设置读取超时为5秒
                conn.setConnectTimeout(10000);// 设置连接网络超时为10秒
                conn.setDoOutput(true);// 设置此方法,允许向服务器输出内容

                // post请求的参数
                String data = content;
                // 获得一个输出流,向服务器写数据,默认情况下,系统不允许向服务器输出内容
                OutputStream out = conn.getOutputStream();// 获得一个输出流,向服务器写数据
                out.write(data.getBytes());
                out.flush();
                out.close();

                int responseCode = conn.getResponseCode();// 调用此方法就不必再使用conn.connect()方法
                if (responseCode == 200) {

                    InputStream is = conn.getInputStream();
                    String response = getStringFromInputStream(is);
                    return response;
                } else {
                    throw new NetworkErrorException("response status is "+responseCode);
                }

            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                if (conn != null) {
                    conn.disconnect();// 关闭连接
                }
            }

            return null;
        }

        public static String get(String url) {
            HttpURLConnection conn = null;
            try {
                // 利用string url构建URL对象
                URL mURL = new URL(url);
                conn = (HttpURLConnection) mURL.openConnection();

                conn.setRequestMethod("GET");
                conn.setReadTimeout(5000);
                conn.setConnectTimeout(10000);

                int responseCode = conn.getResponseCode();
                if (responseCode == 200) {

                    InputStream is = conn.getInputStream();
                    String response = getStringFromInputStream(is);
                    return response;
                } else {
                    throw new NetworkErrorException("response status is "+responseCode);
                }

            } catch (Exception e) {
                e.printStackTrace();
            } finally {

                if (conn != null) {
                    conn.disconnect();
                }
            }

            return null;
        }

        private static String getStringFromInputStream(InputStream is)
                throws IOException {
            ByteArrayOutputStream os = new ByteArrayOutputStream();
            // 模板代码 必须熟练
            byte[] buffer = new byte[1024];
            int len = -1;
            while ((len = is.read(buffer)) != -1) {
                os.write(buffer, 0, len);
            }
            is.close();
            String state = os.toString();// 把流中的数据转换成字符串,采用的编码是utf-8(模拟器默认编码)
            os.close();
            return state;
        }
    }

一定要注意网络权限！

     <uses-permission android:name="android.permission.INTERNET"/>

##3.Volley&OkHttp


Volley&OkHttp应该是现在最常用的网络请求库。用法也非常相似。
都是用构造请求加入**请求队列**的方式管理网络请求。

Volley既可以访问网络取得数据，也可以加载图片，它非常适合数据量不大，但通信频繁的网络操作。对于下载文件这些大数据量的网络操作，它就不适合了。

不过再怎么封装Volley在功能拓展性上始终无法与OkHttp相比。
Volley停止了更新，而OkHttp得到了官方的认可，并在不断优化。

