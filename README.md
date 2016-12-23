
```
function doTerminate(){
    phrowser_obj.terminate(function(){
        process.exit();
    });
}

var phrowser_obj = new Phrowser({},function(){
        phrowser_obj.plugin.on_open=function(pkg){
            phrowser_obj.PH_webpage_obj.evaluate(//grab the first load instance
                function(){return  (new XMLSerializer().serializeToString(document.doctype))+"\n"+document.documentElement.innerHTML+"\n"+"</html>";},
                function(htmlSource){
                    phrowser_obj.stop();//don't load anymore
                    doTerminate();
                }
            );

            var local_open=function(){
                var open_url='https://api.somesite.com',
                    open_opts={
                        'method':'GET',
                        'referer': (utils.basic_str(referer)?referer:''),
                        'data':{
                            'COOKIES':(typeof(cookie_vars)==='object'?cookie_vars:'')
                        }
                    };
                phrowser_obj.open(open_url,open_opts);

            };
            if(phrowser_obj.PH_webpage_obj===false){
                phrowser_obj.init_page(local_open);
            }else{
                local_open();
            }

        });
    });

```
