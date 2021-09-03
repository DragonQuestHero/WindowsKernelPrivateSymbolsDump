# WindowsKernelPrivateSymbolsDump

#### 相当一部分windows内核结构体并不会出现在公有符号中

#### 列如_DEBUG_OBJECT结构 该结构在windows公有符号中无法搜索

<h1 align="center">
	<img src="1.jpg" >
	<br>
	<br>
</h1>

#### 而实际上 该结构在win7 7600中为

typedef struct _DEBUG_OBJECT
{
	/* 0x0000 */ struct _KEVENT EventsPresent;
	/* 0x0018 */ struct _FAST_MUTEX Mutex;
	/* 0x0050 */ struct _LIST_ENTRY EventList;
	/* 0x0060 */ unsigned long Flags;
	/* 0x0064 */ long __PADDING__[1];
} DEBUG_OBJECT, *PDEBUG_OBJECT; /* size: 0x0068 */

#### 使用windbg加载私有符号进行验证

<h1 align="center">
	<img src="1.jpg" >
	<br>
	<br>
</h1>

#### 同理还有更多结构体被隐藏 他们均不会出现在公共符号中 如果用IDA加载私有符号 则情况如下(IDA伪代码未做任何修改 只是单纯的加载私有NT内核符号)

<h1 align="center">
	<img src="3.jpg" >
	<br>
	<br>
</h1>

<h1 align="center">
	<img src="4.jpg" >
	<br>
	<br>
</h1>

### 转储来源项目:https://github.com/wbenny/pdbex (该项目转储自带部分BUG 对私有符号兼容性有一定问题 所以部分结构可能出现小差错)