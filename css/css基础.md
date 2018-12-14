### 背景
#### background-color：
作用：设置元素的背景颜色   
默认：transparent(透明，既无颜色)   
可用值：rgb，rgba，hls，hlsa，keyword(颜色名，transparent)   
#### background-image：
作用：设置元素的背景图片   
默认：none   
可用值：none，url(路径)   
注意：可以设置多个图片，按照z顺序绘画，后面的画在上面，background-color画在背景图片下面   
#### background-position：   
作用：设置元素背景图片的位置   
默认：0% 0%   
可用值：数值，keyword( left | center | right | top | bottom)，百分比   
注意：这些位置都是图片的左上角相对于元素左上角的   
#### background-repeat：   
作用：设置元素背景图片的重复方式   
默认：repeat   
可用值：no-repeat，repeat-x，repeat-y，repeat，space，round   
注意：repeat-x，repeat-y，repeat这三者图片都会被裁减，space 图像会尽可能的重复, 但是不会裁剪，当剩余的空间不够一张图片时，剩余空间均匀分布；round是指容器空间增大时，图片会延展，直到能再放下一个图片时恢复。   
#### background-size：  
作用：设置背景图片大小  
默认：auto auto   
可用值：<length>/<percentage>/auto/cover   
#### background-attachment：   
作用：设置元素背景图片的滚动方式   
默认：scroll   
可用值：fixed(视口固定，及背景图片固定，即使内容滚动背景也不会滚动)，local(背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动)，scroll(背景相对于元素本身固定， 而不是随着它的内容滚动)
#### background：   
作用：一次性集中定义各种背景属性，包括 color, image, origin 与 size, repeat 方式等   
默认：对每个属性都生效，可以自行组合   
多张图片时，用逗号隔开设置对应属性
#### background-clip   
作用：设置元素的背景（背景图片或颜色）是否延伸到边框下面   
默认：border-box   
可用值：border-box，padding-box，content-box，text(只有chrome支持)   
注意：都是延展到相应外延位置的下面，如border是到边框外延，padding是到padding外延

### 边框
#### border简写属性允许你一次将所有的这些都设置在四个边(trbl)   
border-width, border-style, border-color分别仅设置边界的厚度／风格／颜色，并应用到全部四边边界(trbl)
#### border-style   
作用：设置元素边框的样式   
默认：none   
可用值：none，hidden，dotted，dashed，solid，double，round   
注意：可以应用到全部四边边界(trbl)，如border-left-style
#### border-radius   
作用：设置元素边框的圆角   
默认：0   
可用值：长度，百分比(表示圆角半径)   
注意：
1. 可以应用到全部四边边界(trbl)圆角，包括四个属性 border-top-left-radius、border-top-right-radius、border-bottom-right-radius，和 border-bottom-left-radius，空格隔开，一般写1、2或4个值，分别对应全部取一个值，top-left-and-bottom-right、top-right-and-bottom-left分别取其中一个值或四个值一一对应
/* 1st value is top left and bottom right corners,
   		2nd value is top right and bottom left  */
border-radius: 20px 10px;
/* 1st value is top left corner, 2nd value is top right
   		and bottom left, 3rd value is bottom right  */
border-radius: 20px 10px 50px;
/* top left, top right, bottom right, bottom left */
border-radius: 20px 10px 50px 0;
2. 每个圆角包括横竖两个半径，用/隔开；书写格式为 10px 8px / 6px 7px，无/隔开时，横竖半径相等
3. 百分比时分别对应盒模型的长宽的百分比
#### border-image-source   
作用：设置元素边框的边框图片来源   
默认：none   
可用值：url<image>   
注意：与background-image相同
#### border-image-slice   
作用：设置元素边框的边框图片的裁切的像素值   
默认：none   
可用值：百分比，数字
注意：
1. 最多四个值，对应下面图片，取值规则：
两个值:上和下，左和右。
三个值:上、左和右、下。
四个值:上、右、下、左。
2. 该属性将图片切成九宫格，如上图片，该属性会固定四个角，然后以拉长、渐变等处理四个边和中间区域
#### border-image-width   
作用：设置元素边框图片宽度   
默认：border-width   
可用值：length，percentage，number，auto   
注意：
假如border-image-width大于已指定的border-width，那么它将向内部(padding/content)扩展
#### border-image-repeat   
作用：设置元素边框图片的填充方式   
默认：stretch   
可用值：stretch，repeat ，number，round ，space   
注意：
分为水平和垂直两个方向，单个值时，水平和垂直取相同值；两个值时，水平和垂直分别取值
#### border-image   
复合属性包括border-image-source border-image-slice  border-image-width border-image-outset border-image-repeat   
注意：
1. 使用 border-image 时，其将会替换掉 border-style 属性所设置的边框样式
2. border-image相当于给border加上背景图片，类似于background
### 文本
#### color：  
作用：设置文本颜色   
默认：black   
可用值：数值，颜色名，rgb，rgba，hbga
#### text-decoration:   
作用：设置文本修饰方式   
默认：underline   
可用值：none，linethrough，overline，underline   
注意：一般是去除a链接的下划线
#### text-align:
作用：设置文本对齐方式  
默认：underline   
可用值：left，right，center，justify
注意：justify: 使文本展开，改变单词之间的差距，使所有文本行的宽度相同。
#### text-transform:
作用：设置文本字母大小写   
默认：underline   
可用值：uppercase，lowercase，capitalize   
注意：uppercase，lowercase，capitalize用于所有语句的大小写和首字母大写.
#### text-indent:
作用：设置文本首行缩进  
默认：underline  
可用值：uppercase，lowercase，capitalize  
注意：uppercase，lowercase，capitalize用于所有语句的大小写和首字母大写
#### line-height:
作用：设置文每行之间的高  
默认：normal   
可用值：normal，数字，长度，百分比   
注意：
1. 数字时，效果是数字乘以font-size；百分比与元素的字体大小有关；
2. 该属性可以用来使文字居中(当值与height相等时)
#### letter-spacing/word-spacing：  
作用：设置文本中的字母与字母之间的间距、或是字与字之间的间距   
默认：normal   
可用值：normal，长度，百分比   
注意：使用较少，一半用来使文字更易于阅读
#### text-shadow：  
作用：设置文本阴影   
默认：none   
可用值：<color> <offset-x>  <offset-y> <blur-radius>   
注意：
1. 四个值拼接使用，颜色值可以放于任何位置，三个单位依次取，不足则后面的取0；
2. 多个shadow值用逗号隔开
### 字体
#### font-family：   
作用：设置字体类型   
默认：serif, sans-serif, monospace, cursive   
可用值：各种自定义或可用资源字体   
注意：可以设置多个字体，用逗号隔开，当前一个字体找不到时会自动往后找
#### font-size：   
作用：设置字体大小   
默认：medium  
可用值：描述字，数字+单位，百分比   
注意：字体大小单位有px，em，rem；1em 等于我们设计的当前元素的父元素上设置的字体大小
#### font-style：   
作用：设置字体形式   
默认：normal   
可用值：italic，oblique
#### font-weight：   
作用：设置字体粗体大小   
默认：normal   
可用值：bold，lighter， bolder
#### @font-face：导入字体
 {   
  [ font-family: <family-name>; ] ||   
  [ src: [ <url> [ format(<string>#) ]? | <font-face-name> ]#; ] ||   
  [ unicode-range: <urange>#; ] ||   
  [ font-variant: <font-variant>; ] ||   
  [ font-feature-settings: normal | <feature-tag-value>#; ] ||   
  [ font-variation-settings: normal | [ <string> <number>] # ||   
  [ font-stretch: <font-stretch>; ] ||   
  [ font-weight: <weight>; ] ||   
  [ font-style: <style>; ]   
}   
font-family：所指定的字体名字将会被用于font或font-family属性( i.e. font-family: <family-name>; )
src：字体来源，url或本地地址
font-系列：设置字体初始样式

### 列表
#### list-style-type
作用：设置列表的项目符号的类型   
默认：disc  
可用值：circle，square，decimal，lower-roman，lower-alpha....   
注意:start可以设置数字的起始值，reversed设置增长顺序，value可以为每个li设置左侧标记值
#### list-style-image   
作用：设置列表的左侧图片，类似于background-image   
#### list-style-position    
作用：设置在每个项目开始之前，项目符号是出现在列表项内，还是出现在其外   
默认：outside   
可用值: inside ， outside
#### list-style
list-style-type，list-style-image，list-style-position 的组合   
属性值可以任意顺序排列，你可以设置一个，两个或者三个值