## 数据类型

- 整数类型：tinyint, smallint, mediumint, int, bigint

    - zerofill: 可以为整数类型指定宽度。int(3)

- 实数类型：float, double, decimal

    - decimal可存储比bigint还大的整数；可以用于存储精确的小数
    
- 字符串类型:varchar, char, text, blob

    - varchar用于存储可变长度的字符串类型，它比定长类型更节省空间；只会往小了缩，不会扩大

    - varchar超出指定长度会被截断
    
    - char定长，会根据需要采用空格进行填充以方便比较
    
    - char适合存储很短的字符串，或者所有值都接近
    
    - char长度，超出设定的长度，会被截断
    
    - 经常变更的数据，char比varchar更好，char不容易产生碎片
    
    - 对于非常短的列，char比vrachar在存储空间上更有效率
    
    - 尽量避免使用blob/text类型，查询会使用临时表，导致性能开销
    
- 枚举

    - 有时可以使用枚举代替常用的字符串类型
    
    - 把不重复的集合存储成一个预定义的集合
    
    - 非常紧凑，把列表值压缩到一个或两个字节
    
    - 内部存储的是整数
    
    - 尽量避免使用数字作为ENUM枚举的常量，易混乱
    
    - 排序是按照内部存储的整数进行排序
    
    - 枚举表会使表大小大大减小
    
- 日期和时间类型

    - 尽量使用timestamp，比datetime空间效率高
    
    - 用整数保存时间戳的格式通常不方便处理
    
    - 如果需要存储微秒，可以使用bigint存储
    
- 列属性

    - `auto_increment, default, not null, zerofill`
    



































