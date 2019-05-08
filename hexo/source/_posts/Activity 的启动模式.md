### Activity 的启动模式
   Activity 作为四大组件之首，实在是太重要了。而且，Activity的启动模式是个难点，那是因为Activity的四种启动模式和标志位很容易引起混淆，那下面我们就来好好的来分析他，一点一点啃掉这块骨头。^ - ^
 
   为什么goole要求我们使用启动模式呢，由于我们的需求多样化，用户的体验感，以及产品经理的强制要求，真是为难我们开发者了。但是为了我们产品的用户体验感我们还得去做，goole老大哥为了我们开发者能搞出多样化的需求，就设定了Activity的启动模式。好了，步入正题。
   
··· 我们来理解一下Activity什么怎么被系统管理的：管理Activity的是用栈，如果学习过《数据结构》的同学应该都理解。下面我们给出一个图
![栈图](https://user-gold-cdn.xitu.io/2017/11/14/15fb89e045bd4da2?w=698&h=732&f=png&s=35440)

栈只有出口和入口共同用一个，意思就是同一时刻做一个操作，他的原理就是先进后出，后进先出。那么我们创建Activity时就会把这个Activity的实例插入到这个栈中，当我们销毁一个Activity时就会从这个栈中pop掉。如果这个栈中没有任何Activity的实例时，系统就会回收这个栈。目前，goole老大哥给出的有四种启动模式：1.standard 2.singleTop 3.singleTask 4.singleInstance.下面来介绍一下。
#### standard 启动模式

*   标准模式，每次启动一个Activity都会创建一个实例插入栈中，不管栈中是否有这个Activity的实例都会创建，如：栈中有ABCDEF，那么我们启动一个标号为F的Activity时，还是会创建一个F的Activity实例插入到栈中，及栈中最后有这些元素：ABCDEFF。也就是这个F Activity会跑完整的生命周期。有一点需要说明的是，栈是不是随便生成的。比如，当我们启动 A Activity时，如果没有A所需的任务栈，那么就会创建一个一个任务栈，把A插入到任务栈中，如果现在我们要启动B，而B的启动模式时标准模式，那么B会插入到A的那个任务栈中，B就在栈顶了，此时我们销毁B，B就会从任务栈中推出，此时A在栈顶了，A也就可见了。是不是很好理解。** 在我们入门中，会出现一些误区，就拿Context来说吧，Context官方解释时上下文对象，但是Context有ApplicationContext和ActivityContext，当我们使用ApplicationContext去启动一个启动模式时标准模式的Activity时，这时就会报错，因为ApplicationContext并没有任务栈，当我们启动一个标准模式的Activity时，Activity默认会进入启动他的Activity所属的任务栈中。那么我们怎么处理这个问题呢，给这个Activity指定一个标志，FLAG_ACTIVITY_NEW_TASK ,这样Activity其实启动模式时singleTask。
#### singleTop启动模式

*   俗称栈顶复用模式，所谓栈顶复用就是如果我们需要启动的Activity这个实例在栈中并且在栈的顶部，那么就可以拿来用，不用在去创建了。那么我们得注意了，如果样启动Activity，我们的Activity是不会跑完整的生命周期的，像onCreate onStart 不会被调用的，因为他是没有被改变的，他会调用onNewIntent方法，那么我们就可以做区别了，可以在这个方法中判断启动我的是那个，从什么地方来，有因必有果。举个例子吧如：我们的栈中有ABCDEF，那么，当我们的F是使用singleTop启动模式，那么当我们再次启动F时，F并不会创建，而是直接使用栈中的F那么最终我们的栈还是ABCDEF。

#### singleTask启动模式

*  俗称栈内复用，意思就是只要这个栈中有这个实例，我们就拿来用，不用再去创建，如果这个实例没有在栈顶，我们系统会把他调到栈顶来。这就是我们日常开发所写的单例模式差不多，终于找到感觉了。如栈中有ABCDEF实例，那么我们的B设定的是singleTask模式，现在我们去启动B时，B就会调到栈顶来，栈就变成这个样了：ACDEFB。是不是很好理解。

#### singleInstance启动模式

*  这也是一种单例模式，只不过他与singleTask不同的是，这个模式是会单独创建一个栈来放这个Activity，也就是singleTask的加强版本。

介绍了这四种启动模式，那么我们怎么在开发中使用呢，有两种，
1.在AndroidMenifest中设定
<pre>
<code>
android:name=".ar.arvideo.ArVideoActivity"
android:configChanges="screenLayout"
android:launchMode="singleTask"
</code>
</pre>
2.在代码中设置
<pre>
<code>
Intent intent=new Intent();
intent.setClass(this,FaceTestActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
</code>
</pre>
第二种的优先级比第一种优先级高。
下面介绍一些Activity的Flags

1.  FLAG_ACTIVITY_NEW_TASK：
作用是指定Activity的启动模式为“singleTask”

2.  FLAG_ACTIVITY_SINGLE_TOP:
设置Activity的启动模式为“singleTop”
3.  FLAG_ACTIVITY_CLEAR_TOP：
意思就是使用这个标记位时，处于这个Activity以上的都会被清除栈，这个要和“singleTask”一起使用。

-------
好了内容就介绍到这里了，有什么不足之处，希望大家指出，小强同志


