最近项目要往 Mecab 分词器里添加一些自定义新词,看了好多日文教程,走了好多弯路.关于Mecab的中文教程还是挺少的,暂且写下一点自己的经验.

往Mecab添加自定义新词有两种方法:1.加入系统字典,2.加入用户字典

#### 1.加入系统字典:
  适用于不频繁更新字典,不想解析速度变慢的情况
  
#### 2.加入用户字典:
  更新系统字典比较花时间;如果需要频繁更新字典,或者没有操作系统字典的权限时,选择添加新的用户字典比较好.
  
这里我采用的是 2 的方法,因为想单独创建一个与我项目有关的单词的字典

#### 步骤:
1.创建 cvs 文件
2.计算 cost
3.编译字典

#### 1.创建 cvs 文件
  Mecab 的字典都是采用.cvs 格式文件.
  在哪里创建 .cvs 文件不重要.
  添加新词的格式:
  >表層形,左文脈ID,右文脈ID,コスト,品詞,品詞細分類1,品詞細分類2,品詞細分類3,活用型,活用形,原形,読み,発音
  
  例:
  >1号館,-1,-1,1,名詞,固有名詞,一般, * , * , * , 1号館,イチゴウカン,イチゴーカン,(テストユーザ辞書)

很多教程的例中品词分类是这种形式:
> 固有名词, * , * , * , * ,

但现在固有名詞的右側的*已经改成了"一般",不然编译时会报下面的错:

> reading ./test.csv ... context_id.cpp(96) [it != left_.end()] cannot find LEFT-ID  for 名詞,固有名詞,*,*,*,*,*

#### 2.计算 cost
其实 cost 值可以不用太在意.cost 越小说明这个词越常见,越大说明越少见.只要不是设置得特别大,就没问题

#### 3.编译字典
最重要的一步

把刚才创建的文件复制到 mecab 存放字典的目录(里面有各种各样自带的.cvs 文件)中,网上教程中的路径都写的/usr/local/lib/mecab/dic/,但我编译时一直报错,
所以复制到原有的字典目录下是肯定不会错的

然后就可以开始编译了.编译软件是 mecab 自带的mecab-dict-index,在 finder 搜索里面找到这个文件,做好准备

打开终端(mac 环境下),cd进入刚刚的字典目录之后,把mecab-dict-index拖进终端里,然后按以下的形式输入命令:

> mecab-dict-index -f <csvファイルの文字コード> -t <辞書ファイルの文字コード>

因为是日语,所以都是utf-8,即:

> mecab-dict-index -f utf-8 -t utf-8

网上参考的 windows 环境中的路径:

> c:\MeCab\bin>mecab-dict-index.exe -f shift-jis -t utf8

然后静静等待done 的出现就完成了...









