![Version](https://img.shields.io/badge/version-1.0.1-red)
![Version](https://img.shields.io/badge/milestones-2-brightgreen)


# Calculation_Test: 加减法测试app

<img src="app/src/main/res/picture/01.jpeg" width="30.5%" height="30.5%">
<img src="app/src/main/res/picture/02.jpeg" width="30.5%" height="30.5%">

<br>

### 结构
MainActivity用来写按下返回键的逻辑  
MyViewModel用来存各种String和LiveData，以及随机生成加减法函数的逻辑  
TitleFragment用来写主页的逻辑  
QuestionFragment用来写计算过程（键盘输入、回答正确、回答错误）的逻辑  
WinFragment用来写win界面的逻辑  
LoseFragment用来写lose界面的逻辑  
layout文件夹下包含activity_main、fragment_title、fragment_question、fragment_win、fragment_lose等等布局  


<br>
<br>

### 细节
#### 1.  创建MyViewModel.java，使用ViewModel构建项目    
SaveStateHandle处理String  
SharedPreferences设置初值  
定义MutableLiveData  
随机生成加法减法的逻辑：  
```
void generator() {
        int LEVEL = 20;
        Random random = new Random();
        int x, y;
        x = random.nextInt(LEVEL) + 1;
        y = random.nextInt(LEVEL) + 1;
        if (x % 2 == 0) {
            getOperator().setValue("+");
            if (x > y) {
                getAnswer().setValue(x);
                getLeftNumber().setValue(y);
                getRightNumber().setValue(x - y);
            } else {
                getAnswer().setValue(y);
                getLeftNumber().setValue(x);
                getRightNumber().setValue(y - x);
            }

        } else {
            getOperator().setValue("-");
            if (x > y) {
                getAnswer().setValue(x - y);
                getLeftNumber().setValue(x);
                getRightNumber().setValue(y);
            } else {
                getAnswer().setValue(y - x);
                getLeftNumber().setValue(y);
                getRightNumber().setValue(x);
            }
        }
    }
```
保存结果的逻辑 save() 函数  
更新结果的逻辑 answerCorrect() 函数 （设置flag）


<br>

#### 2. 重写onCreate方法

<br>

初始化binding用于访问xml的UI元件（View对象）  
`binding = ActivityMainBinding.inflate(layoutInflater)`

<br>

设置root（根/所有）将内容视图设置为activity（活动）的布局的根视图
`setContentView(binding.root)`

<br>

在按钮上设置点击监听器来执行计算小费的方法 calculateTip()
```
binding.button.setOnClickListener {
            calculateTip()
        }
```
<br>
<br>

#### 3.写明计算小费的方法 calculateTip()  
得到输入的金额转化为成本cost  
根据选择的按钮获取百分比  
小费 = 成本 * 百分比 (tip = cost * tipPercentage)  
四舍五入或取整数roundUp  
展示小费的金额  
隐藏键盘  
