# 創一個臨時表單並傳送


有點忘記是在寫什麼，只能先記錄下來。    
<!--more-->

```javascript
 $("#apply").click(function () {
        post("@Url.Action("ParentChildActivity")#profile", { "pfid": "@Model.pfid", "cate": "@Model.cate" })
    });

    function post(url, params) {
        var temp_form = document.createElement("form");

        temp_form.action = url;
        temp_form.target = "_self";
        temp_form.method = "post";
        temp_form.style.display = "none";

        for (var x in params) {
            var opt = document.createElement("input");
            opt.name = x;
            opt.value = params[x];
            temp_form.appendChild(opt);
        }
        document.body.appendChild(temp_form);
        temp_form.submit();
    }
```
