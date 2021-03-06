# 主要类型

---

下面列出了与该类型相关的主要类型和附加信息和其他字节

* 主要类型 0：一个无符号整数。5-bit附加信息要么是整数本身\(额外的信息值0-23\)，要么是附加数据的长度。附加信息值24意味着整数用一个额外的uint8\_t来表示，25表示一个uint26\_t，26表示一个uint32\_t，27表示一个uint64\_t。例如，整数10表示为1字节0b000\_01010\(主要类型0，附加信息10\)。整数500表示为0b00011001\(主要类型为0，附加信息25\)，后面是两个字节0x01f4，这是十进制的500。
* 主要类型1：一个负整数。编码规则遵循\(主要类型0\)，除了值是绝对值-1。例如，整数-500是0b001\_11001（主要类型1，附加信息25），后面是两个字节0x01f3，它是十进制的499。
* 主要类型2：一个字节流。字节串的字节长度遵循\(主要类型0\)，例如，一个字节串的长度是5，初始字节为0b010\_00101\(主要类型2，附加信息5表示长度\)，紧跟着5个字节的二进制内容。一个字节的长度是500，3个初始字节，0b010\_11001\(主要类型2，附加信息25指示一个两字节的长度\)，紧跟着两个字节0x01f4表示长度500，紧跟着500个字节的二进制内容。
* 主要类型3：一个文本字符串，指定一串Unicode字符，这些字符编码为UTF-8 \[RFC3629\]。格式与主要类型2相同，长度给出字节数。这个类型为了提供可显示的、人类可读文本字符串。并且允许对具体非结构性编码和指定编码类型的文本。与JSON等格式不同，这种类型的Unicode字符永远不会被转义。因此，换行符\(U+000 A\)总是以字串表示为字节0x0a。而不是字节0x5c6e\(字符“\”和“n”\)或0x5c7530303061\(字符“\”、“u”、“0”、“0”、“0”和“a”\)。
* 主要类型4：一组数据项。数组也称为列表、序列或元组。数组的长度遵循字节字符串的规则\(主要类型2\)，除了长度表示数据项的数量，没有长度字节表示数组占用字节数。数组中的项目不需要都是相同类型的。例如，包含10个任何类型的项的数组初始字节为0b100\_01010\(主要类型为4，额外信息为10的长度\)，后面是10个剩余的项。
* 主要类型5：一组数据项的映射\(k-v键值对\)。映射也被称为表、字典、散列或对象\(在JSON中\).映射由kv键值对数据项组成，每一个kv键值对包含一个key，后面直接跟着value。映射的长度遵循\(主要类型2\)的规则，除了长度表示键值对的数量外，没有长度字节表示映射占用字节数。例如，一个映射包含9个键值对初始字节为0b100\_01001\(主要类型为5，额外信息为键值对的数量9\)，后面是18个的数据项。第一个项是第一个key，第二个项是第一个value，第三个项是第二个key，以此类推。映射里面重复的key可能符合格式，但它不是有效的，因此会导致不确定的解码。详情见 3.7章节。
* 主要类型6：其他主要类型的可选语义标记\(扩展\)，详情2.4章 \(日期时间、URI、URL、大数\)
* 主要类型7：主要类型7用于两种类型的数据:浮点数和不需要任何内容的“简单值”。在初始字节中，5位附加信息的每个值都有其各自的含义，如表1中所定义的那样。就像整数的主要类型一样，这个主要类型的项目不包含内容数据;所有信息都在初始字节中。



