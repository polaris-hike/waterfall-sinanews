## 懒加载和瀑布流布局
预览地址：https://wuxuweilalala.github.io/waterfall-sinanews/index.html
## 懒加载原理
### 什么是图片懒加载？
所谓图片懒加载是当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成一张大小为1*1px图片的路径（这样就只需请求一次），只有当图片出现在浏览器的可视区域内时，才设置图片正真的路径，让图片显示出来。
### 为什么要使用图片懒加载？
比如一个页面中有很多图片，如淘宝、京东首页等等，如果一上来就发送这么多请求，页面加载就会很漫长，如果js文件都放在了文档的底部，恰巧页面的头部又依赖这个js文件，那就不好办了。更为要命的是：一上来就发送百八十个请求，服务器可能就吃不消了（又不是只有一两个人在访问这个页面）。
因此优点就很明显了：不仅可以减轻服务器的压力，而且可以让加载好的页面更快地呈现在用户面前（用户体验好）。
### 如何实现？
把所有img的src设置为一张图片，这样网络请求只用走一次。
当页面加载之后，检查图片，1.看是不是出现在用户的视野之中，2.如果是就加载真实的图片。
```
    start()
    $(window).on('scroll', function(){
        start()
    })
    
    function start(){
        $('.container img').not('[data-isLoaded]').each(function(){
            var $node = $(this)
            if( isShow($node) ){
                loadImg($node)
        }            
         })
    }
    
    
    // 判断元素是否在页面内，若是返回ture，不是返回false
    function isShow($node){
      return $node.offset().top <= $(window).height() + $(window).scrollTop()
    }
    // 加载图片，
    function loadImg($img){
      $img.attr('src', $img.attr('data-src'))
      $img.attr('data-isLoaded', 1)  //函数节流
    }
```
## 瀑布流原理
- 瀑布流布局要求要进行布置的元素等宽，然后计算元素的宽度与浏览器宽度之比，得到需要布置的列数。
- 创建一个数组，长度为列数，里面的值为已布置元素的总高度（最开始为0）
- 然后将未布置的元素依次布置到高度最小的那一列，就得到了瀑布流布局。
## 实现原理
1. 获取page=1 的10条数据
2. 把十条数据拼装成dom 放到页面
3. 使用瀑布流去摆放 dom位置
4. page++

当页面滚动时
1. 获取page 的10条数据
2. 把十条数据拼装成dom 放到页面
3. 使用瀑布流去摆放 dom位置
4. page++
