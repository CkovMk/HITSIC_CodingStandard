# 代码规范 Beta 2

by: C.M.    2019.09.09



## 目录

### 第零部分 绪论

一、 Beta 1 版本绪论

### 第一部分 语法规范

一、头文件与源文件

二、 变量

​	2.1. 变量命名

​		保留变量名

​		一般命名

​		前缀与后缀

​	2.2. 变量的使用

​		全局变量

​		静态变量

​		局部变量

​		指针与引用

​		常量与宏

​		枚举类型

​	2.3. 变量注释

三、 表达式和语句

四、 函数

​	5.1. 保留函数命名

五、 结构体与类

六、 预编译指令

七、 异常处理

​	7.1. 通用异常处理

​	7.2. C语言异常处理

​	7.3. C++异常处理



### 第二部分 架构规范

一、 顶层架构

​	1.1. 设计概述

​	1.2. 运行时环境（RTE）

​	1.3. 系统驱动层（DRV）

​	1.4. 应用软件层（APP）

二、 设计模式

​	2.1. 单件模式

​	2.2. 观察者模式

​	2.3. 状态模式

​	2.4. 命令模式

​	2.5. 责任链模式

​	2.6. 解释器模式

三、 框架规范

​	3.1. 驱动系统层接口

​	3.2. 应用软件层框架

​	



## 总论

### 一、 Beta 1版本绪论

建立完备的编码规则，有助于提高代码的可读性、可维护性、可移植性。代码规范，已经成为具有一定规模的开发团队的必需品。我认为，对于眼下规模的智能车社团，建立一套科学高效、易于遵守的代码规范，可以提高各队之间合作的效率，也便于进行技术交流与传承。基于以上考量，我开始着手制定本代码规范。

本规范的制定，参考了UNIX和GNU的代码规范，结合NXP MCUxpresso SDK中的代码，同时借鉴了本人了解的一些大型项目（包括但不仅限于U8glib、Amazon FreeRTOS）的代码。由于规范的制定免不了会受个人习惯和偏好的影响，可能会影响规范的科学性，因此也欢迎各位通过实践找出其中的不完善之处。

代码规范并不会让写代码变得更简单，恰恰相反，遵守代码规范在大多数时候都是一项令人恼火的负担。但这种所谓的负担，却在无形之中减少了了开发团队的内耗，降低了沟通成本，提高了合作的效率。无数的实践已经证明，高效的开发需要统一的标准。对于社团而言，代码规范的建立绝非一日之功，它需要各位的严格遵守，需要经年累月的不断完善，甚至可能需要几代智能车人的坚守。是时候放弃旧有的野蛮生长的代码方式了。我衷心的希望，在经历了转变的阵痛之后，智能车的开发会变得更加简单高效，我们将有机会在有限的时间内做到更多，智能车的未来可以成就更多辉煌。

根据计划，本代码规范包括两部分：语法规范和架构规范。语法规范主要包括了以下内容：

- 不同情况下该使用何种语法
- 各种语法元素在不同情况下的命名方式及注释内容

而架构规范主要包括：

- 工程架构应当如何分层
- 常见的功能模块应该如何书写
- 一些用于实现特定功能的固定结构形式（设计模式）

本代码规范将使用以下词汇描述对规范约束的严格程度：

- 必须：任何情况下都要这样做，须严格遵守。
- 应该：大多数情况下应该这样做，特殊情况在知晓其危害的前提下可以违背该规范。
- 推荐：建议的做法，不遵守该规范可能会使得代码的编写变得困难。
- 不推荐：不建议的做法，不遵守该规范可能会使得代码的编写变得困难。
- 避免：大多数情况下不应该这样做，特殊情况在知晓其危害的前提下可以违背该规范。
- 禁止：任何情况下都不要这样做，须严格遵守。

目前在编写的是第一部分。

C.M.@hit    2019.09.09



### 二、 Beta 2 版本绪论

Beta 1版本的代码规范主要编写的内容是第一部分，随着第一部分即语法规范的完善，其涉及的内容也更加广泛。其中部分内容已经开始涉及一些模式和架构的话题。因此在Beta 1版本的基础上，我开始编写第二部分，这就是Beta 2版本。必须承认，第一部分的内容远远称不上完善，可能存在一些漏洞和问题，也有一些规则值得商榷。在这一版本中，我希望能通过架构规范的不断完善，发现一些潜藏在代码规范中的问题，并加以修复。在架构规范部分，我在制定规范之余，也会根据规范修改完善现有代码，同时不断验证规范的严谨性。





## 第一部分 语法规范



### 一、 头文件与源文件

#### 1.1. 文件规范

1. 所有文件必须采用UTF-8编码格式。

2. 文件命名以`所属层级`+`_`+`文件名主体`构成。有时还包括文件名后缀。文件名一律采用小写英文字符。

   > 示例1：
   >
   > ```C++
   > #include "rte_common.hpp"		//Runtime Environment Layer, common
   > #include "drv_pitmgr.hpp"		//System Driver Layer, PIT Manager
   > #include "drv_disp_st7789.hpp"	//System Driver Layer, Display Driver, Model "ST7789"
   > #include "app_ctrl.hpp"			//Application Layer, Controll
   > ```
   >
   > 具体的分层结构详见**“第二部分 架构规范”**。

3. 工程中的头文件（`*.h`、`*.hpp`）与源文件（`*.c`、`*.cpp`）推荐以相同文件名成对出现。其中头文件可以独立出现而不与源文件成对，而除了`main.c/main.cpp`（用于保存主程序）和`isr.c`（用于保存中断服务程序）两个源文件外，其他源文件禁止独立出现。

4. 包含C++代码的头文件和源文件，必须以`*.hpp`、`*.cpp`作为文件扩展名。

5. 多数情况下，源文件（`*.c`、`*.cpp`）中仅允许包含（`#include`）与自身同名的头文件（`*.h`、`*.hpp`），且C语言文件禁止包含含有C++代码的文件。极少数情况下，如果某个头文件中仅包含定义时即被初始化的变量，则源文件中可以包含这类头文件，但必须保证该头文件**仅在此源文件中包含一次**。

   > 示例1：
   > ```c++
   > /* appimg.hpp */
   > #pragma once
   > #include "include.h"	//general include
   > #include "drvcam.h"		//camera driver
   > 
   > //other code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appimg_iptmap.hpp */
   > static uint8_t appimg_iptMap[IMG_IPT_R][IMG_IPT_C] = 
   > {
   > //here comes IMG_IPT_R * IMG_IPT_C numbers...  
   > };
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appimg.cpp */
   > #pragma once
   > #include "appimg.hpp"
   > #include "appimg_iptmap.hpp"
   > 
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   > 上述代码演示了1.1.5所述的规范。假设`appimg.hpp`与`appimg.cpp`是图像处理的头文件与源文件，而`appimg_iptmap.hpp`内存储了图像处理所需的逆透视矩阵。逆透视矩阵有数百行，上万个参数，由算法生成，如果直接放在`appimg.cpp`中，会占用大量空间。相信没有人会希望要查找代码时里面混杂着几百行全是数组初始化参数的文件，因此我们把这部分数据单独写在一个文件中。这样做的另一个原因是，如果我们希望修改该逆透视矩阵，可以在电脑上用C语言程序修改参数后，利用文件读写功能直接输出文件，然后直接替换，完全不需要编辑代码。但是，如果我们在一个文件中包含两次`appimg_iptmap.hpp`，则会出现重定义错误。因此，对此文件的包含仅能在appimg.cpp中出现一次，不可在其他地方再次包含。
   >
   > 这种情况还有另一个解决方案：
   >
   > 示例2：
   >
   > ```c++
   > /* appimg.hpp */
   > #pragma once
   > #include "include.h"	//general include
   > #include "drvcam.h"		//camera driver
   > #include "appimg_iptmap.hpp"
   > 
   > extern uint8_t appimg_iptMap[IMG_IPT_R][IMG_IPT_C]
   > //other code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appimg_iptmap.cpp */
   > uint8_t appimg_iptMap[IMG_IPT_R][IMG_IPT_C] = 
   > {
   > //here comes IMG_IPT_R * IMG_IPT_C numbers...  
   > };
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appimg.cpp */
   > #pragma once
   > #include "appimg.hpp"
   > 
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   >
   > 这违反了规范1.1.3，故应避免这种写法。

6. 推荐将所有经常包含的头文件分类放在2~3个头文件中，方便包含。推荐的方案为：一个头文件用于存放C语言标准库（如果是C++工程，需要两个头文件将标准库与C++标准库分开存放，避免C文件不小心包含了C++头文件导致无法找到头文件的错误），一个头文件用于存放NXP MCUXpresso SDK的头文件，一个头文件用于存放和开发平台无关的定义（如图像大小、PI等常量、常用的数据类型和数据结构定义等）。



#### 1.2. 格式规范

1. Pending： 文件头注释，代码块注释

   ```c++
   /* @BetaCat_Ctrl v2.1
    * @use this is a simple demostration of File Comments.
    * @auth Chekhov
    * @date 2019.09.10
    * @v1.00
    */
   ```

2. Pending：空行 空格 。。。。。。







### 二、 变量

#### 2.1. 变量命名

- ##### 保留变量名


1. 禁止使用`i`、`j`、`k`、`x`、`y`、`z`、`m`、`n`、`p`、`r`、`c`等具有数学含义、物理含义或现实意义的字母或单词作为全局变量名。
2. 禁止使用形如`aa`、`bbb`等毫无意义的名称作为变量名。
3. 避免使用形式常被用于系统保留的字符作为变量名，如全部大写（`NAME`）、首尾带下划线（`_name_`、`_name`、`name_`、`__name__`、`__name`、`name__`）。



- ##### 一般命名


4. 全局变量命名采用`小写前缀`+ `_`+`Camel命名法`。

   > 示例1：
   >
   > ```c++
   > /* drvimu.cpp */
   > 
   > uint8_t drvimu_dataBuf[14];
   > int16_t drvimu_rawAx,drvimu_rawAy,drvimu_rawAz; 
   > int16_t drvimu_rawGx,drvimu_rawGy,drvimu_rawGz;
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```

5. 局部变量和函数内的静态（`static`）变量直接采用`Camel命名法`。



- ##### 前缀与后缀


6. 全局变量的前缀应与文件名或文件名主体保持一致。若文件名太长，可一定程度缩写。前缀一般不少于3字符，不多于8字符。



#### 2.2. 变量的使用

1. 声明变量时，必须对变量进行初始化。禁止默认未初始化的变量值为0的行为。禁止用未初始化的变量参与运算。

- ##### 全局变量


2. 在C++代码中，应避免使用全局变量。在C语言中，也应该谨慎使用全局变量。

3. 避免以全局变量的形式在模块之间传递变量。如要在模块之间传递变量，应该通过传递指针或传递引用的形式。

   > 示例1：
   >
   > `drvimu.hpp`与`drvimu.cpp`是IMU驱动模块，用于读取六轴传感器的三轴加速度、角速度等数据。函数`void DRVIMU_GetAcceData(void)`的功能是读取IMU的三轴加速度。`appctrl.cpp`是控制代码模块，函数`void APPCTRL_urAngCtrl(void);`的作用进行平衡车的角度控制。为了获取车模当前角度，需要读取IMU。
   >
   > ```c++
   > /* drvimu.hpp */
   > //ohter code...
   > extern int16_t drvimu_rawAx, drvimu_rawAy, drvimu_rawAz;
   > 
   > void DRVIMU_GetAcceData(void);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* drvimu.cpp */
   > //ohter code...
   > int16_t drvimu_rawAx,drvimu_rawAy,drvimu_rawAz;
   > 
   > void DRVIMU_GetAcceData(void)
   > {
   >  drvimu_rawAx = DRVIMU_getAx();
   >  drvimu_rawAy = DRVIMU_getAy();
   >  drvimu_rawAz = DRVIMU_getAz();
   > }
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appctrl.cpp */
   > //ohter code...
   > void APPCTRL_angCtrl(void)
   > {
   >  extern int16_t drvimu_rawAx, drvimu_rawAy, drvimu_rawAz;
   >  DRVIMU_GetAcceData();
   >  //use these varibles directly here.
   >  //...
   > }
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   >
   > 上述代码就违背了本条规范。我们在模块`drvimu`内定义了相关变量，然后在`appctrl`模块内直接引用了上述变量，逻辑上并没有任何问题。问题在于在IMU的驱动程序完成之前，写控制的人事先并不知道这些变量的名字，而必须在IMU驱动写完后与写IMU驱动的人交流。如果类似的情况频繁发生，写控制的人就不得不就许多变量进行询问，大大降低了开发效率。更可怕的是，如果开发IMU驱动的人更改了变量名称，或是换用了不同人写的不同型号IMU的驱动程序，上层程序将不得不对所有用到这些变量的代码进行修改，工程量之大显而易见。这些损耗都是不必要的，可以通过下面的方式加以避免：
   >
   > 示例2：
   >
   > ```C++
   > /* drvimu.hpp */
   > //ohter code...
   > 
   > void DRVIMU_GetAcceData(int16_t& drvimu_rawAx, int16_t& drvimu_rawAy, int16_t& drvimu_rawAz);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* drvimu.cpp */
   > //ohter code...
   > 
   > void DRVIMU_GetAcceData(int16_t& drvimu_rawAx, int16_t& drvimu_rawAy, int16_t& drvimu_rawAz);
   > {
   >  drvimu_rawAx = DRVIMU_getAx();
   >  drvimu_rawAy = DRVIMU_getAy();
   >  drvimu_rawAz = DRVIMU_getAz();
   > }
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* appctrl.cpp */
   > //ohter code...
   > int16_t appctrl_rawAx, appctrl_rawAy, appctrl_rawAz;
   > 
   > void APPCTRL_angCtrl(void)
   > {
   >  
   >  DRVIMU_GetAcceData(appctrl_rawAx, appctrl_rawAy, appctrl_rawAz);
   >  //...
   > }
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   > 这种方式完美地规避了前面提到的问题。底层驱动提供的接口需要调用它的上层程序传递给底层驱动变量的引用，这样底层驱动就可以把读到的数据放到上层程序指定的地方。上面的程序基于C++编写，C语言没有“引用”的概念，只需把C++的引用换成C语言的指针即可。实际上，“引用”就是对指针的一次包装。这是我们第一次接触“接口”的概念。接口，可以简单地定义成**“用于访问一类数据的统一方法”**。因此，这条规范也可以写成：**“以函数为接口，而不以变量为接口”**。
   >
   
4. 仅在一个模块内使用的全局变量，推荐在源文件内定义，而不在对应头文件中进行`extern`声明。只有需要外部访问的全局变量，才在头文件中以`extern`修饰声明。

- ##### 静态变量

5. 仅在一个函数内使用的全局变量，应该使用函数内的静态（static）变量。

6. 不建议在头文件中定义变量。如果不得不在头文件中定义变量（不加`extern`的情况），则必须以static修饰，且禁止重复包含。这种情况往往出现在一个模块只有头文件、没有对应源文件的情况。

7. 对C++广义类（`class`和`struct`）中的常量和由所有对象共享的属性，应当使用静态成员变量。

- ##### 局部变量

- ##### 指针与引用

- ##### 常量与宏

8. 对于单片机编程，不经特殊处理的情况下，所有常量（字面值和声明值）位于程序存储器。一般来说，MK66、KV58等普通单片机的程序存储器I/O性能弱于随机存储器。在这类单片机上，由于性能考量，即使一个需要频繁访问的变量在逻辑上是常量，也不应该以`const`修饰或使用宏定义。

- ##### 枚举类型

9. 对于具有互斥属性或特定含义的一组数据，应该使用枚举类型（`enum`）加以描述。

10. 对于C11或C++11及更新的标准C/++，使用枚举类型（`enum`）时必须指明所用的存储类型。C语言在声明全局枚举类型时，其内部的枚举项必须含有与枚举类型名相同的前缀。C++11及更新标准的C++，在声明全局枚举类型时，必须用`enum class`的形式声明，而不必带有前缀；在声明作用域内的枚举类型时，如果作用域范围较大，也应该使用`enum class`的形式。

   > 示例1：
   >
   > ```c++
   > /* test1.h */
   > //C11 enum
   > #include <stdint.h>
   > enum testC_t: uint8_t
   > {
   > 	testC_A,
   > 	testC_B,
   > 	testC_C,
   > };
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > /* test2.hpp */
   > #include <cstdint>
   > //C++14 global enum class
   > enum class testCpp_t : uint8_t
   > {
   > 	A,
   > 	B,
   > 	C,
   > };
   > 
   > //C++14 regional enum 
   > class myclass
   > {
   > public:
   >     enum testCpp_t : uint8_t
   >     {
   >         A,
   >         B,
   >         C,
   >     };
   > };
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```

- ###### 常量枚举

11. 当所表示的数据为一系列具有相似含义的数（如某I2C设备的寄存器地址）时，必须采用枚举类型。这种用法称为常量枚举。

   > 示例1：
   >
   > ```c++
   > /** Integration time settings for TCS34725 */
   > typedef enum : uint8_t
   > {
   >   TCS34725_INTEGRATIONTIME_2_4MS =
   >       0xFF, /**<  2.4ms - 1 cycle    - Max Count: 1024  */
   >   TCS34725_INTEGRATIONTIME_24MS =
   >       0xF6, /**<  24ms  - 10 cycles  - Max Count: 10240 */
   >   TCS34725_INTEGRATIONTIME_50MS =
   >       0xEB, /**<  50ms  - 20 cycles  - Max Count: 20480 */
   >   TCS34725_INTEGRATIONTIME_101MS =
   >       0xD5, /**<  101ms - 42 cycles  - Max Count: 43008 */
   >   TCS34725_INTEGRATIONTIME_154MS =
   >       0xC0, /**<  154ms - 64 cycles  - Max Count: 65535 */
   >   TCS34725_INTEGRATIONTIME_700MS =
   >       0x00 /**<  700ms - 256 cycles - Max Count: 65535 */
   > } tcs_integrationTime_t;
   > ```
   >
   > 这是用于表示TCS34725传感器积分时间的寄存器取值的`enum`。

- ###### 标志位枚举

12. 当所表示的数据为按位表示的逻辑（如某flag的各位代表的含义）时，必须采用枚举类型。这种用法称为标志位枚举。

   > 示例1：
   >
   > ```c++
   > enum class ctrl_excp_t : uint32_t
   > {
   > 	camLostWay = 1 << 0,
   > 	magLostWay = 1 << 1,
   > 	cam_cross = 1 << 2,
   > 	mag_cross = 1 << 3,
   > 	cam_loopEntry = 1 << 4,
   > 	cam_loopEnter = 1 << 5,
   > 	cam_inLoop = (cam_loopEntry | cam_loopEnter),
   > 	mag_loopEntry = 1 << 6,
   > 	mag_loopEnter = 1 << 7,
   > 	mag_inLoop = (mag_loopEntry | mag_loopEnter),
   > 	loopL = 1 << 8,
   > 	loopR = 1 << 9,
   > 	whichLoop = (loopL | loopR),
   > 	rampRoad = 1 << 10,
   > 	hinder = 1 < 11,
   > 	snapRoad = 1 << 12,
   > 	//add more if needed.
   > };
   > ```
   >
   > 这是一个用于表示各种标志位的`enum class`。



#### 2.3. 变量注释







### 三、 表达式和语句

#### 3.1. 表达式

1. 对于语义不明的表达式，增加括号以确定运算顺序。



#### 3.2. 条件语句

1. if和switch-case语句必须覆盖变量的所有可能性。如果某些分支逻辑上永远不可能出现，使用`else`或`default:`分支以捕获异常。

> 示例1：
>
> ```c++
> int sw = 0;
> 
> // some code to change sw value...
> 
> if(sw < 10)
> {
>     printf("SW < 10\n");
> }
> else if (sw < 20)
> {
>     printf("10 <= SW < 20\n");
> }
> else
> {
>     printf("Error!\n");
> }
> 
> 
> switch(sw)
> {
>     case 0:
>         printf("sw = 0\n");
>         break;
>     case 1:
>         printf("sw = 1\n");
>         break;
>     case 2:
>         printf("sw = 2\n");
>         break;
>     default:
>         printf("sw Out of Range!\n");
>         break;
> }
> 
> ```
>
> 





#### 3.3. 循环语句







### 四、 函数

#### 4.1. 函数命名

- ##### 保留函数命名


1. 禁止使用`i`、`j`、`k`、`x`、`y`、`z`、`m`、`n`、`p`、`r`、`c`等具有数学含义、物理含义或现实意义的字母或单词作为函数名。

2. 禁止使用形如`aa`、`bbb`等毫无意义的名称作为函数名。

3. 避免使用形式常被用于系统保留的字符作为函数名，如全部大写（`NAME`）、首尾带下划线（`_name_`、`_name`、`name_`、`__name__`、`__name`、`name__`）。

4. 禁止使用容易误解为变量名的名称作为函数名。

5. 必须规范使用某些具有特定含义的函数名，如下所示：

   > 示例1：
   >
   > ```c++
   > void PREFIX_Init(void);				//initliazation
   > void PREFIX_Setup(void);
   > void PREFIX_Deinit(void);
   > //unfinished.
   > ```
   >
   > Init保留字表示初始化。DeInit保留字表示取消初始化。Setup表示设置，与Init的别是Init往往用于对硬件操作，而Setup用于对内存中的变量进行初始化，而无需硬件操作。

   如果工程内有其他约定的特定含义的符号，也应遵循本条规范。

- ##### 一般命名


6. 头文件和源文件中定义的可被外部调用的函数，命名采用`大写前缀`+ `_`+`Pascal命名法`。

   > 示例1：
   >
   > ```c++
   > /* drvimu.cpp */
   > //ohter code...
   > status_t DRVIMU_Init(void);
   > status_t DRVIMU_GetAcceData(int16_t& drvimu_rawAx, int16_t& drvimu_rawAy, int16_t& drvimu_rawAz);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```

7. 头文件和源文件中定义的不可外部调用的函数，命名采用`大写前缀`+ `_`+`Camel命名法`。

   > 示例1：
   >
   > ```c++
   > /* drvimu.hpp */
   > //ohter code...
   > void DRVIMU_getConfig(drvimu_cfg_t& cfg);
   > status_t DRVIMU_i2cRx(uint8_t addr, uint8_t reg, uint16_t size, uint8_t* data);
   > status_t DRVIMU_i2cTx(uint8_t addr, uint8_t reg, uint16_t size, uint8_t* data);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```

- 
  ##### 前缀与后缀


8. 函数的前缀应与文件名或文件名主体保持一致。若文件名太长，可一定程度缩写。前缀一般不少于3字符，不多于8字符。

9. 函数可以带有用于说明不同版本或特性的后缀，采用Pascal命名法，以下划线连接。

   > 示例1：
   >
   > ```c++
   > /* drvftm.hpp */
   > //ohter code...
   > void DRVFTM_MotorUpdate(int16_t pwmL, int16_t pwmR);
   > void DRVFTM_MotorUpdate_HiRes(float pwmL, float pwmR);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   >
   > MotorUpdate是用于更新电动机PWM占空比、设置电机功率的函数，有两个版本：普通版本（`MotorUpdate`）接受[-100，100]的整形参数，高精度版本（`MotorUpdate_HiRes`）接受[-100.0,100.0]的浮点参数。



#### 4.2. 函数的使用

- ##### 内联函数

1. 当一个函数较短且频繁调用时，建议使用内联函数。内联函数可以节约用于函数跳转的时间，提高执行效率。内联函数的函数体需要写在头文件中。

   > 示例1：
   >
   > ```c++
   > /* drvftm.hpp */
   > //ohter code...
   > inline void DRVFTM_MotorUpdate(int16_t pwmL, int16_t pwmR)
   > {
   >     //...
   > }
   > inline void DRVFTM_MotorUpdate_HiRes(float pwmL, float pwmR)
   > {
   >     //...
   > }
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```

   

- ##### 静态函数

2. 当一个函数被频繁调用时，建议使用静态函数。静态函数将永久地驻留内存，使得调用该函数时无需开辟新的栈空间，提高执行效率。

- ##### 函数的返回值

3. 必须谨慎使用函数返回值。任何存在异常状态的函数，都必须具有`status_t`类型的返回值，用于表示该函数执行的状态。函数正确执行必须返回0。除非有充分的理由（例如适配器函数等特殊情况 ），否则禁止将函数返回值用于传递结果。如要返回结果，应该在参数中添加用于保存结果的指针或引用。

   > 示例1
   >
   > ```C++
   > /* drvimu.hpp */
   > //ohter code...
   > drvimu_cfg_t DRVIMU_getConfig(void);
   > uint8_t DRVIMU_i2cRxReg(uint8_t addr, uint8_t reg);
   > void DRVIMU_i2cTxReg(uint8_t addr, uint8_t reg, uint8_t data);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   >
   > 上述代码违背了本条规范。
   >
   > 第一个函数用于获得IMU的默认配置，其将一个较大的结构体直接作为函数的返回值。这在一定程度上可以被看作是适配器函数，这样写虽然不推荐但问题不大。
   >
   > 第二个函数用于从IMU的寄存器读取一个字节，它将读取的结果直接作为返回值返回。先不说它与下面的写寄存器函数缺乏对称性，如果I2C读取过程中出现了错误，底层API给出了非0的异常返回值，该函数必须自行处理（事实上这是不可能的），而无法将该错误返回给上层。此时上层函数大概率会得到返回值0，从而无法区分究竟是该函数发生了错误还是该寄存器的值为0。
   >
   > 第三个函数用于向IMU的寄存器写一个字节。这种写法的问题在于无法返回错误，上面已经写得很清楚了，不再赘述。
   >
   > 示例2：
   >
   > ```C++
   > /* drvimu.hpp */
   > //ohter code...
   > void DRVIMU_getConfig(drvimu_cfg_t& cfg);
   > status_t DRVIMU_i2cRxReg(uint8_t addr, uint8_t reg, uint8_t data);
   > status_t DRVIMU_i2cTxReg(uint8_t addr, uint8_t reg, uint8_t data);
   > //ohter code...
   > 
   > /* ---------- ---------- ---------- ---------- */
   > ```
   >
   > 以上是正确示例。

- 
  ##### 函数的参数

- 
  ##### 函数重载




#### 4.3. 函数注释

1. 注释格式：`@brief`用于说明函数的主要功能；`@param`用于说明各参数的作用。

   > 示例1：
   >
   > ```c++
   > /* fsl_cmp.h */
   > 
   > /*!
   >  * @brief Enables/disables the DMA request for rising/falling events.
   >  *
   >  * This function enables/disables the DMA request for rising/falling events. Either event triggers the generation of
   >  * the DMA request from CMP if the DMA feature is enabled. Both events are ignored for generating the DMA request from the CMP
   >  * if the DMA is disabled.
   >  *
   >  * @param base CMP peripheral base address.
   >  * @param enable Enables or disables the feature.
   >  */
   > void CMP_EnableDMA(CMP_Type *base, bool enable);
   > ```
   >
   > 该示例来自K66的NXP官方SDK。

2. 所有可外部调用的函数必须含有完善的注释。内部函数按需。









### 五、 结构体与类

#### 5.1. 结构体与类命名

- ##### 保留命名




- ##### 一般命名


1. 全局结构体与类命名采用`小写前缀`+ `_`+`Camel命名法`+`_t`。
2. 作用域内的结构体与类命名采用`Camel命名法`+`_t`。



- ##### 成员变量命名








- ##### 成员函数命名






#### 5.2. （C语言）结构体的使用

1. 对于C语言中需要创建多个实例的项目，应将每个实例共有的数据结构抽象为结构体，并提供一组函数用于操作这些数据结构。

> 示例1：
>
> 参见K66 MCUXpresso SDK中的各种Handle。



#### 5.3. （C++语言）类的使用

1. 对于C++中的任何功能相对独立的模块，创建类以界定其数据结构和方法的作用域。如果能且仅能创建一个实例，使用单件模式。如果能且仅能创建指定个数的实例，使用改进的单件模式（多件模式）。

> 示例1：单件模式的一种实现
>
> 这是用户界面UI的显示适配器的部分代码。由于通常情况下我们仅拥有一块屏幕，我们不希望用户随意创建该显示适配器的实例，故使用单件模式。单件模式的实现有很多种，这里按照需要取其中一种：
>
> ```c++
> /* app_ui.hpp */
> 
> //display HAL
> class appui_disp_t
> {
> public:
> 	static appui_disp_t& GetInst(void)
> 	{
> 		static appui_disp_t inst;
> 		return inst;
> 	}
>  
> 	//...
> 
> private:
> 	appui_disp_t(void) {}
> 	appui_disp_t(const appui_disp_t&);
> 	appui_disp_t& operator = (const appui_disp_t&);
> };
> 
> 
> ```
>
> 首先，将该类的默认构造函数、拷贝构造函数、赋值运算符定义为私有，从而不可能在此类作用于外创建该类的实例。然后创建公有静态函数`static appui_disp_t& appui_disp_t::GetInst(void);`，在此函数内创建静态实例`static appui_disp_t inst`。调用该函数时，永远返回对`inst`的引用。
>
> 
>
> 示例2：改进的单件模式（多件模式）
>
> 这是串口驱动的部分代码。此代码共使用三组串口，分别用于调试（`dbugInst`）、无线数传（`wlanInst`）、通信（`intcInst`）。由于串口的使用受到硬件的限制，我们不希望用户随意创建串口对象的实例。而由于需要预先创建的串口实例有三个，前面的单件模式已无法胜任。因此对单件模式做如下改进：
>
> ```C++
> /* drv_uartmgr.hpp */
> 
> class uartmgr_t
> {
> public:
> 	static uartmgr_t& GetInst(LPUART_Type* instNum)
> 	{
> 		static uartmgr_t dbugInst(LPUART1);
> 		static uartmgr_t wlanInst(LPUART4);
> 		static uartmgr_t intcInst(LPUART5);
> 		switch ((uint32_t)instNum)
> 		{
> 		case LPUART1_BASE:
> 			return dbugInst;
> 			break;
> 		case LPUART4_BASE:
> 			return wlanInst;
> 			break;
> 		case LPUART5_BASE:
> 			return intcInst;
> 			break;
> 		default:
> 			return dbugInst;
> 			break;
> 		}
> 		return dbugInst;
> 	}
> 	
> 	//...
> 
> private:
> 
> 	uartmgr_t(LPUART_Type* _base)
> 	{
> 		//...
> 	}
> };
> ```
>
> 首先，将该类的默认构造函数、拷贝构造函数、赋值运算符定义为私有，从而不可能在此类作用于外创建该类的实例。然后创建公有静态函数`static uartmgr_t& uartmgr_t::GetInst(LPUART_Type* instNum);`。在此函数内创建三个静态实例`static uartmgr_t dbugInst(LPUART1);`、`static uartmgr_t wlanInst(LPUART4);`、`static uartmgr_t intcInst(LPUART5);`。调用该函数时，根据参数返回对前述三个静态实例之一的引用。



2. 对于C++中的一组具有相似方法的对象，创建纯虚类作为接口。

> 示例1：
>
> 在编写用户界面中的菜单时，菜单项有很多种：整形变量、浮点变量、子菜单等，甚至可以通过菜单调用函数。这些类型的菜单都具有一组相似的方法，我们把它抽象出来，定义为虚基类。
>
> 注意：**原则上C++用作接口类的基类必须是纯虚基类，不按规定编写可能产生难以预期的后果。**
>
> ```c++
> //UI Menu menuItem interface
> class menuItemIfce_t
> {
> public:
> 	enum type_t : uint8_t
> 	{
> 		nullType,
> 		variType,
> 		varfType,
> 		procType,
> 		menuType,
> 	};
> 	enum message_t : uint32_t
> 	{
> 		selected,
> 		deselected,
> 		dataUpdate,
> 	};
> 	enum propety_t : uint32_t
> 	{
> 		//data config
> 		data_global = 1 << 0,	//data save in global area
> 		data_region = 1 << 1,	//data save in regional area
> 		data_getPos = data_global | data_region,
> 		data_ROFlag = 1 << 2,	//data read only
> 		data_prioRW = 1 << 3,	//data rw prior than other item
> 		data_getCfg = data_global | data_region | data_ROFlag | data_prioRW,
> 
> 		//error mask
> 	};
> 	typedef void (*slotFunction_t)(menuItemIfce_t* _this, message_t _msg);
> 	static uint32_t itemCnt;
> 
> 
> 	type_t type;
> 	menuList_t* myList;
> 	uint32_t pptFlag;	//property flag
> 	uint32_t list_id, unique_id;
> 	std::string nameStr;
> 	slotFunction_t slotFunc;
> 		/*
> 		 * Configure by Constructor Default:
> 		 *     type,unique_id,slotFunc
> 		 * Configure by Constructor Parameter:
> 		 *     pptFlag,nameStr,(*data)
> 		 * Configure by menuList insert() Default:
> 		 *     myList,list_id,
> 		 */
> 
> 	virtual void installSlotFunction(slotFunction_t _func) final { slotFunc = _func; }
> 	virtual void uninstallSlotFunction(void) final { slotFunc = NULL; }
> 	virtual void slotCall(message_t _msg) final
> 	{
> 		if (slotFunc != NULL) { (*slotFunc)(this, _msg); }
> 	}
> 	//used when reading or saving data
> 	virtual uint32_t getData(void) = 0;
> 	virtual void setData(uint32_t _data) = 0;
> 	virtual bool getIndex(menuItemIdex_t* _data) final;
> 	//used when in menuList
> 	virtual void printSlot(appui_menu_t::dispSlot_t _slot) = 0;
> 	virtual void directKeyOp(appVar_keyBTOp_t * _op) = 0;
> 	//used when in menuItem
> 	virtual void printDisp(void) = 0;
> 	virtual void keyOp(appVar_keyBTOp_t * _op) = 0;
> };
> //End of UI Menu menuItem interface
> ```
> 
> 在创建基于上述接口类的子类时，只需公有继承上述接口类：
> 
>```c++
> //UI Menu menuItem menuEntry_type
>class menuItem_menuType_t : public menuItemIfce_t
> {
> public:
> 	menuList_t* data;
> 	menuItem_menuType_t(menuList_t* _data, std::string _nameStr, uint32_t _pptFlag);
> 	~menuItem_menuType_t(void);
> 	//used when reading or saving data
> 	void setData(uint32_t _data) final{}
> 	uint32_t getData(void) final { return  0;/* (uint32_t)data;*/ }
> 	//used when in menuList
> 	void printSlot(appui_menu_t::dispSlot_t _slot) final;
> 	void directKeyOp(appVar_keyBTOp_t* _op) final;
> 	//used when in menuItem
> 	void printDisp(void) final;
> 	void keyOp(appVar_keyBTOp_t* _op) final;
> };
> 	
> //UI Menu menuItem integer_varible
> class menuItem_variType_t : public menuItemIfce_t
> {
> public:
> 	int32_t* data;
> 	int32_t bData;
> 	menuItem_variType_t(int32_t* _data, std::string _nameStr, uint32_t _pptFlag);
> 	~menuItem_variType_t(void);
> 	//used when reading or saving data
> 	void setData(uint32_t _data) final { (*data) = _data; }
> 	uint32_t getData(void) final { return *((uint32_t*)data); }
> 	//used when in menuList
> 	void printSlot(appui_menu_t::dispSlot_t _slot) final;
> 	void directKeyOp(appVar_keyBTOp_t* _op) final;
> 	//used when in menuItem
> 	void printDisp(void) final;
> 	void keyOp(appVar_keyBTOp_t* _op) final;
> private:
> 	static const int32_t lut[4];
> 	int32_t v, e;
> 	int32_t cur;	//cursor pos
> 	void getContent(int32_t& v, int32_t& e, int32_t data);
> 	void setContent(int32_t& data, int32_t v, int32_t e);
> 		
> };
> 
> //UI Menu menuItem floatpoint_varible
> class menuItem_varfType_t : public menuItemIfce_t
> {
> public:
> 	float* data;
> 	float bData;
> 	menuItem_varfType_t(float* _data, std::string _nameStr, uint32_t _pptFlag);
> 	~menuItem_varfType_t(void);
> 	//used when reading or saving data
> 	void setData(uint32_t _data) final { (*data) = *((float*)(&_data)); }
> 	uint32_t getData(void) final { return *((uint32_t*)data); }
> 	//used when in menuList
> 	void printSlot(appui_menu_t::dispSlot_t _slot) final;
> 	void directKeyOp(appVar_keyBTOp_t* _op) final;
> 	//used when in menuItem
> 	void printDisp(void) final;
> 	void keyOp(appVar_keyBTOp_t* _op) final;
> private:
> 	static const int32_t lut[4];
> 	int32_t v, e;
> 	int32_t cur;	//cursor pos
> 	void getContent(int32_t& v, int32_t& e, float data);
> 	void setContent(float& data, int32_t v, int32_t e);
> 
> };
> 
> 
> //UI Menu menuItem process_type
> class menuItem_procType_t : public menuItemIfce_t
> {
> public:
> 	enum cmdFlag_t
> 	{
> 		menu_dircKeyOp_avail = 1 << 0,
> 		menu_itemKeyOp_avail = 1 << 1,
> 		menu_dispRequest = 1 << 2,
> 
> 		proc_exitProcess = 1 << 15,
> 	};
> 	typedef void (*procHandler_t)(appVar_keyBTOp_t* _op,uint32_t& _flag, int32_t& _retv);
> 
> 	int32_t retv;
> 	uint32_t flag;
> 	procHandler_t data;
> 	menuItem_procType_t(procHandler_t _data, std::string _nameStr, uint32_t _pptFlag);
> 	~menuItem_procType_t(void);
> 
> 	//used when reading or saving data
> 	void setData(uint32_t _data) final{}
> 	uint32_t getData(void) final { return 0; }
> 	//used when in menuList
> 	void printSlot(appui_menu_t::dispSlot_t _slot) final;
> 	void directKeyOp(appVar_keyBTOp_t* _op) final;
> 	//used when in menuItem
> 	void printDisp(void) final;
> 	void keyOp(appVar_keyBTOp_t* _op) final;
> };
> ```
> 
> 上面我创建了四种菜单项类型：子菜单类型、整形参数类型、浮点参数类型、函数调用类型。



















### 六、 预编译指令







### 七、 异常处理

#### 7.1. 通用异常处理



#### 7.2. C语言异常处理

鉴于C语言并没有特别完善的异常处理机制，我们只有`assert`一个工具可以使用。在一些关键的地方插入assert语句以检查错误。对于那些`if...else...`和`switch...case...`语句中逻辑上不可能到达的分支，插入`assert(0);`。由于assert失败后会直接调用`abort()`终止程序，这样的错误处理是不可恢复的。

另一种可以借鉴的方式是自己写一套异常处理。我用过的一种方式是利用一个用不到的外设中断（例如K66上的USB中断、没有用到的UART对应的中断、RTC中断等），将其设置为最高优先级（当然你愿意使用ARM内置的Fault中断那就更好了）。构造一个`my_assert`函数，如果`my_assert`失败，则在在指定位置写入一条预先定义的错误信息，然后软件置位前述中断的中断标志位（ARM有提供对应函数，当然你也可以自己操作寄存器）。进入中断服务函数后，可以根据情况执行车模保护（如关闭电机、舵机打角到安全位置、关闭控制环等）、在屏幕或串口打印消息等操作，甚至可以通过按键输入来恢复现场，让车模继续运行。

#### 7.3. C++异常处理

在C++程序中应避免使用`asssert`。C++有完善的异常处理功能，包括`throw`和`try...catch...`。在可能出现问题的代码处，如果判断出现问题，则调用`throw`语句抛出异常（exception）。可以抛出C++内置异常类型，也可以抛出自己定义的异常类型。如果上下文没有异常捕获语句`try...catch...`，throw的作用和assert相同，会终止程序。如果检测到了异常终止，可以加上异常捕获，然后在异常捕获中根据情况执行车模保护、故障信息输出等工作。处理结束后同样可以恢复现场，继续执行。







## 第二部分 架构规范

### 一、顶层架构

#### 1.1. 设计概述

整个架构的顶层设计主要分为三层，由下至上依次为：运行时环境（Runtime Environment，RTE）、系统驱动层（System Driver Layer，DRV），应用软件层（Application Layer，APP）。RTE提供对基础外设的初始化；DRV提供对具体设备的访问、任务间通信、任务管理，并对同一类设备提供接口抽象；应用软件层包含界面绘制、控制逻辑等高端功能



#### 1.2. 运行时环境（RTE）

RTE提供了对基础外设（如PIT、DMA、XBAR、AOI等设备）的初始化与配置。大部分工作都可以由配置工具直接生成，开发者要做的主要是对配置工具生成的代码进行查漏和补全。RTE并不提供对底层外设的访问功能。

一部分RTE内容直接由NXP MCUXpresso实现，无需自行编写。

RTE还提供初始化注册的功能，任何硬件初始化都应在RTE内置位对应标志位。



#### 1.3. 系统驱动层（DRV）

DRV层包括两个部分：系统组件和驱动组件。驱动组件主要提供对具体外设的访问，包括高级外设的初始化、读写操作、控制逻辑。对于一些常用的外设（如IMU，摄像头，电机等），DRV层还应提供统一的接口以供应用层访问。系统组件的功能主要包括：定时任务和事件触发器的管理，处理计时、延迟、中断路由等系统服务，提供异常处理、外部数传逻辑等相关功能。

和电脑上运行的操作系统一样，驱动和系统总是密不可分的，系统必须包含一些自身运行所必须的驱动。因而系统部分的代码往往会包含对所需外设的控制。这是一个介于单片机底层和上层逻辑之间的连接层。而独立于系统部分之外的驱动部分，则对系统部分有较高的依赖性，各种限制也较多。不同驱动之间禁止直接互相通信，而必须通过向系统注册观察者的方式接收通知；驱动层必须能够自动更新相关的数据，并产生对应的事件或消息。

这里系统层的消息架构采用观察者模式。



#### 1.4. 应用软件层（APP）

APP软件通常面向DRV层或APP自身的内部组件工作。对于智能车而言，APP组件通常包括以下几部分：控制逻辑、数据存储、界面与I/O、图像等高端数据处理。一般的，控制逻辑、数据存储、界面与I/O共同构成一个MVC系统，而数据处理往往作为控制逻辑的子系统出现，但工作相对独立，更多依赖于下层驱动的时序。

在这个MVC系统中，数据存储模块集中了控制逻辑所需的全部变量，称为“模型”（Model）。界面与I/O主要包括用户界面逻辑、Flash或SD卡等NV存储、蓝牙串口或Wi-Fi数传的高端逻辑、上位机通信逻辑等，称为“观察者”（Viewer）。控制逻辑主要的作用是对用户操作和传感器状态等输入做出响应，根据这些输入的变量计算输出，被称为“控制器”（Controller）。

我们将在控制逻辑部分采用状态模式，实现一个状态机。

我们将在界面的菜单部分采用命令模式（用于处理业务逻辑）和责任链模式（用于处理操作输入），在Console命令行部分采用解释器。



### 二、 设计模式

#### 2.1. 单件模式

#### 2.2. 观察者模式

#### 2.3. 状态模式

#### 2.4. 命令模式

#### 2.5. 责任链模式

#### 2.6. 解释器模式



### 三、 框架规范

#### 3.1. 驱动系统层接口

1. 定时中断管理器（PITMGR）

   - 概述

     定时中断管理器的模块名为PITMGR，包括两个文件，`pitmgr.hpp`和`pitmgr.cpp`。

     定时中断管理器属于驱动系统层的系统部分，主要通过PIT硬件计时，向系统提供全寿命计时器（Lifetime Counter，LTC）、精准延迟计时、定时任务管理等功能。PIT硬件在PITMGR中初始化和配置。通常情况下配置如下：Chnl0和Chnl1通过ChainMode连接成一个64位定时器，作为LTC。NXP非常贴心地在SDK中为我们准备了读取LTC的函数`uint64_t PIT_GetLifetimeTimerCount(PIT_Type* base);`，可以直接调用。这个LTC同时用作延时功能。Chnl2配置为定时周期为1ms的定时器，开启中断，为系统服务提供节拍。Chnl3按需配置。

   - C语言实现

     `const uint64_t pitmgr_pitClkFreq;` 常量，定义了PIT时钟频率，需要根据具体单片机的时钟配置修改。

     `status_t PITMGR_Init(void);` PIT初始化与配置。

     `uint64_t getLTC(void);`	获取LTC计数。**注意：PIT是自动重装减法计数器。**需要用`ULLONG_MAX-PIT_GetLifetimeTimerCount(PIT);`得到从0开始的计数。

     `uint64_t getLTC_us(void);` 获取LTC计数的微秒数。

     `uint64_t getLTC_ms(void);` 获取LTC计数的毫秒数。

     `void delay(uint64_t _t);` 通过LTC进行时钟级别的延时。

     `void delay_us(uint64_t _t);` 通过LTC进行微秒级延时。

     `void delay_ms(uint64_t _t);` 通过LTC进行毫秒级延时。

     `typedef void (*pitmgr_handler_t)(void);` 定时任务函数指针。

     ```c
     enum pptFlag_t : uint32_t
     {
     	pitmgr_pptEnable = 1 << 0,
     	pitmgr_pptRunOnce = 1 << 1,
     };
     ```

     标志位枚举，PIT系统中断服务的属性flag。`enable`指当前任务的启用状态，`runOnce`指该任务是否运行一次后自动禁用。还可按需添加更多flag。

     

     ```c
     struct pitmgr_handle_t
     {
     	uint32_t ms, mso;
     	pitmgr_handler_t handler;
     	uint32_t pptFlag;
     };
     ```

     任务控制结构体。其中`ms`表示运行周期，`mso`表示运行相位（通俗讲就是该任务会在第`k*ms+mso`毫秒时被执行）。该机制是用于平衡每一个毫秒的负载。假如A任务每2ms运行一次，用时20us，B任务每5ms运行一次，用时100us，则第10毫秒共用时120us。如果这样的累积事件越累越多，有可能导致在特定的节拍处用时超过1ms的定时周期，导致定时周期的不稳定。`handler`用于存储任务的函数指针，`pptFlag`用于存储前面enum所定义的属性flag。

     

     `const uint32_t pitmgr_isrSetSize;` 简单起见，这里不再对系统定时中断服务的数组采用动态内存，而是简单定义一个绝对够用大小。当然也可以自己实现一个链表，这部分可以自由发挥。

     `uint32_t pitmgr_isrSetCnt;` 这个变量用于存储当前系统定时中断服务数组中系统定时中断的个数。

     `pitmgr_handle_t pitmgr_isrSet[pitmgr_isrSetSize];` 这就是系统定时中断服务数组了。

     `uint32_t pitmgr_timer_ms;` 这是PIT中断的毫秒计时变量。亦可在`isr`函数内静态定义。

     `void PITMGR_setup(pitmgr_handle_t* _handle, uint32_t _ms, uint32_t _mso, handler_t _handler, uint32_t _ppt);` 这是一个内部函数，用于设置一个handle。

     `void PITMGR_setEnable(pitmgr_handle_t* _handle, uint8_t _b)` 这是一个内部函数，用于设置一个handle的启用状态。这里uint8_t实际用作bool，因为C语言没有原生的bool（我不喜欢boolean）。

     `pitmgr_handle_t* PITMGR_Insert(uint32_t _ms, uint32_t _mso, handler_t _handler, uint32_t _ppt);` 如果数组未满，这个函数用于向数组末尾插入一个系统定时中断服务，它会使`pitmgr_isrSetCnt`增加1，并返回指向所插入的`pitmgr_handle_t`结构体的指针。如果数组已满，（在DEBUG配置下）应当`assert`失败，并（在RELEASE配置下）返回`NULL`。注：性能考虑，`assert`在RELEASE配置下往往被忽略。**注意：该函数需要暂时关闭PIT中断。**

     `status_t PITMGR_Remove(pitmgr_handle_t* _handle );` 该函数为可选函数，用于删除一个特定的系统定时中断任务。由于采用了数组的存储方式，删除非末尾的节点可能存在困难。该函数返回操作的结果。对于需要频繁开关的系统定时中断任务，建议使用`pptFlag`设置启用和禁用，而不是反复添加和删除任务。

     `void PITMGR_Isr(void);` 该函数为本模块的中断处理函数，中断服务函数将调用此函数来实现对系统定时中断任务的处理。此函数用于处理PITMGR内部的业务逻辑，而并不负责清除PIT硬件的标志位。标志位由PIT的IRQHandler清除。

     

   - C++实现

     ```c++
class pitmgr_t
     {
     public:
     
     	//LifeTimeCounter
     	static uint64_t getLTC(void);
     	static uint64_t getLTC_us(void);
     	static uint64_t getLTC_ms(void);
     	//delay using LifeTimeCounter
     	static void delay(uint64_t _t);
     	static void delay_us(uint64_t _t);
     	static void delay_ms(uint64_t _t);
     
     	//isr service manager
     	typedef void (*handler_t)(void);
     	enum pptFlag_t : uint32_t
     	{
     		enable = 1 << 0,
     		runOnce = 1 << 1,
     		//msgWake = 1 << 2,
     	};
     	static std::list<pitmgr_t> isrSet;
     	static uint32_t timer_ms;
     
     	static pitmgr_t& insert(uint32_t _ms, uint32_t _mso, handler_t _handler, uint32_t _ppt);
         static status_t remove(pitmgr_t& _handle);
     	static void isr(void);
     
     	//isr service content
     	uint32_t ms, mso;
     	handler_t handler;
     	uint32_t pptFlag;
     
     	void setup(uint32_t _ms, uint32_t _mso, handler_t _handler, uint32_t _ppt);
     	void setEnable(bool _b);
     	pitmgr_t(void);
     private:
     
     	pitmgr_t(uint32_t _ms, uint32_t _mso, handler_t _handler, uint32_t _ppt);
     
     };
     ```
     
     
     
     
     
     

 

2. 外部中断管理器（EXTMGR）

   - 概述
   - C语言实现
   - C++实现

   

3. 串口管理器（UARTMGR）

   - 概述
   - C++实现

4. I2C通用接口与SPI通用接口

   

5. 电机与舵机驱动接口（MOTOR/SERVO）

   

6. IMU惯导驱动接口（DRVIMU）

   - 概述

     

   - C语言实现

     

   - C++实现

     

   

7. 显示屏幕接口（DISP）

   



#### 3.2. 应用软件层框架

1. 顶层MVC框架

   

2. 数据存储框架

   

3. 控制状态机框架

   

4. NV存储接口（NVSTO）

   

5. 界面显示适配器（APPUI）

   

6. 菜单逻辑接口（APPUI_MENU）

   

7. 日志与命令行接口（APPUI_DLOG）

   

8. 图像显示接口（APPUI_IMAGE）

   

9. 通用数传接口







