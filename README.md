# embed-size
Ví dụ về tự động thay đổi chiều cao embed iframe theo nội dung iframe cross domain

Trong parent.htm
 ```
<html>
<head>
    <meta charset="utf-8">
    <script>

        window.addEventListener("message", function (event) {
            //console.log("A nhận được", event);
            //Nếu iframe postMessage có dữ liệu dạng {type:'embed-size',height:'document.body.scrollHeight',href: window.location.href}
            if (typeof (event.data) === 'object'
                && event.data.type === "embed-size"
                && typeof (event.data.href) == "string"
                && event.data.href.length > 0) {
                //tìm đến các iframe có src = event.data.href bao gồm cả src tương đối và tuyệt đối 
                document.querySelectorAll('iframe[src="' + event.data.href + '"],iframe[src="' + event.data.href.slice(location.origin.length) + '"]').forEach(function (o) {
                    o.style.height = event.data.height + "px";//set lại chiều cao của iframe
                });
                console.log(event.data.href,event.data.href);
            }
        }, false);

    </script>
</head>
<body>
    <p>Chiều cao iframe tự động được resize lại theo nội dung trong iframe</p> 
    <iframe src="/iframe.htm" height="200"></iframe>
</body>
</html>
 ```
   Trong iframe.htm
 ```
<html>
<head>
    <meta charset="utf-8">
    <script>
        window.addEventListener("load", function () {
            if (window !== window.parent) {
                //Gửi thông điệp đến parent để parent chỉnh lại chiều cao khi được embed bằng iframe
                window.parent.postMessage({
                    type: 'embed-size',
                    height: document.body.scrollHeight,
                    href: window.location.href
                }, "*");
            }
        });
    </script>
</head>
<body>
    <div id="lipsum">
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. In euismod lacus ut congue auctor. Fusce tincidunt aliquet odio, at hendrerit velit bibendum id. Etiam ullamcorper auctor nulla, et pellentesque mi consequat vel. Donec blandit semper justo porttitor bibendum. Donec id mauris posuere, finibus mi id, pulvinar tortor. Sed tempus risus ut iaculis tincidunt. Mauris a orci posuere, venenatis arcu nec, fermentum mi. Etiam aliquam sollicitudin ligula, ut hendrerit leo suscipit ac. Suspendisse eleifend vestibulum diam, vel lobortis quam laoreet a.
        </p>
        <p>
            Aliquam dui nisl, volutpat vel mi vitae, sagittis malesuada tellus. Phasellus lacinia vestibulum eros, eget venenatis ipsum lacinia facilisis. Aliquam in magna at dui cursus interdum at in lectus. Nam id felis sed nunc sollicitudin fringilla eu auctor metus. Curabitur pharetra porta leo sodales ultrices. Pellentesque cursus dolor id ultrices dapibus. Sed eleifend leo sed libero cursus, in posuere dui faucibus. Vivamus dignissim tincidunt metus. Phasellus vitae tincidunt nulla. Donec lacinia nisl sem, id laoreet nunc lobortis et. Suspendisse id tortor quis elit vulputate ornare eu non dolor. Proin maximus diam eu tempor maximus. In a felis quis ligula semper rutrum ac sit amet lectus. Cras posuere, arcu eu pretium pharetra, ante sem sollicitudin diam, in mattis nulla metus nec libero.
        </p>
    </div>
</body>
</html>
 ```
