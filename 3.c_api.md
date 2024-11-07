# C++存储过程接口（C++开发存储过程接口）

## base64_encode
Base64编码和解码。简单实现，用于BLOB字段。如果性能很重要，请寻找优化后的实现。

### namespace lgraph_api
### namespace base64

#### Functions

```cpp
inline std::string Encode(const char *p, size_t s)
```
Encodes a string to Base64.  
将字符串编码为Base64。

**参数**  
p – 要编码的字符串。  
s – 字符串的大小。  

**返回**  
The encoded string.  
编码后的字符串。

```cpp
inline std::string Encode(const std::string &str)
```
Encodes a string to Base64.  
将字符串编码为Base64。

**参数**  
str – 要编码的字符串。  

**返回**  
The decoded string.  
编码后的字符串。

```cpp
inline bool TryDecode(const char *p, size_t s, std::string &ret)
```
Tries to decode a Base64 string.  
尝试解码Base64字符串。

**参数**  
p – 要解码的字符串。  
s – 字符串的大小。  
ret – [out] 解码后的字符串。  

**返回**  
True if the string was decoded successfully, false otherwise.  
如果字符串成功解码则返回true，否则返回false。

```cpp
inline bool TryDecode(const std::string &str, std::string &out)
```
Tries to decode a Base64 string.  
尝试解码Base64字符串。

**参数**  
str – 要解码的字符串。  
out – [out] 解码后的字符串。  

**返回**  
True if the string was decoded successfully, false otherwise.  
如果字符串成功解码则返回true，否则返回false。

```cpp
inline std::string Decode(const char *p, size_t s)
```
Decode a Base64 string and throw exception if fails.  
解码Base64字符串，如果失败则抛出异常。

**抛出**  
InputError – 如果字符串不是有效的Base64字符串，则抛出。

**参数**  
p – 要解码的字符串。  
s – 字符串的大小。  

**返回**  
The decoded string.  
解码后的字符串。

```cpp
inline std::string Decode(const std::string &str)
```
Decode a Base64 string and throw exception if fails.  
解码Base64字符串，如果失败则抛出异常。

**抛出**  
InputError – 如果字符串不是有效的Base64字符串，则抛出。

**参数**  
str – 要解码的字符串。  

**返回**  
The decoded string.  
解码后的字符串。







## lgraph

### Defines

#### LGAPI
namespace lgraph_api

### Typedefs

```cpp
typedef bool GetSignature(SigSpec &sig_spec) 
```
这行代码定义了一个函数指针类型 `GetSignature`，它接受一个 `SigSpec` 类型的引用作为参数，并返回一个布尔值。

```cpp
typedef bool Process(lgraph_api::GraphDB &db, const std::string &input, std::string &output) 
```
这行代码定义了一个函数指针类型 `Process`，它接受一个 `lgraph_api::GraphDB` 的引用、一个常量字符串和一个字符串的引用作为参数，并返回一个布尔值。

```cpp
typedef bool ProcessInTxn(lgraph_api::Transaction &txn, const std::string &input, lgraph_api::Result &output) 
```
这行代码定义了一个函数指针类型 `ProcessInTxn`，它接受一个 `lgraph_api::Transaction` 的引用、一个常量字符串和一个 `lgraph_api::Result` 的引用作为参数，并返回一个布尔值。










## lgraph_atomic
Implementation of atomic operations, used in lgraph_traversal.
实现原子操作，用于 lgraph_traversal。

### namespace lgraph_api

### Functions

```cpp
template<class T>
inline bool cas(T *ptr, T oldv, T newv) 
```
这个模板函数定义了一个原子 Compare-And-Swap 操作，接受指针 `ptr`、旧值 `oldv` 和新值 `newv` 作为参数。

```cpp
template<class T>
inline bool write_min(T *a, T b) 
```
这个模板函数定义了一个原子最小写操作，接受指针 `a` 和一个值 `b`，将 `a` 的值更新为 `b` 和 `*a` 中的较小值。

```cpp
template<class T>
inline bool write_max(T *a, T b) 
```
这个模板函数定义了一个原子最大写操作，接受指针 `a` 和一个值 `b`，将 `a` 的值更新为 `b` 和 `*a` 中的较大值。

```cpp
template<class T>
inline void write_add(T *a, T b) 
```
这个模板函数定义了一个原子加法操作，将值 `b` 添加到指针 `a` 指向的值上。

```cpp
inline void write_add(uint64_t *a, uint64_t b) 
```
这个函数定义了一个针对 `uint64_t` 类型的原子加法操作，将值 `b` 添加到指针 `a` 指向的值上。

```cpp
inline void write_add(uint32_t *a, uint32_t b) 
```
这个函数定义了一个针对 `uint32_t` 类型的原子加法操作，将值 `b` 添加到指针 `a` 指向的值上。

```cpp
inline void write_add(int64_t *a, int64_t b) 
```
这个函数定义了一个针对 `int64_t` 类型的原子加法操作，将值 `b` 添加到指针 `a` 指向的值上。

```cpp
inline void write_add(int32_t *a, int32_t b) 
```
这个函数定义了一个针对 `int32_t` 类型的原子加法操作，将值 `b` 添加到指针 `a` 指向的值上。

```cpp
template<class T>
inline void write_sub(T *a, T b) 
```
这个模板函数定义了一个原子减法操作，将值 `b` 从指针 `a` 指向的值中减去。

```cpp
inline void write_sub(uint64_t *a, uint64_t b) 
```
这个函数定义了一个针对 `uint64_t` 类型的原子减法操作，从指针 `a` 指向的值中减去 `b`。

```cpp
inline void write_sub(uint32_t *a, uint32_t b) 
```
这个函数定义了一个针对 `uint32_t` 类型的原子减法操作，从指针 `a` 指向的值中减去 `b`。

```cpp
inline void write_sub(int64_t *a, int64_t b) 
```
这个函数定义了一个针对 `int64_t` 类型的原子减法操作，从指针 `a` 指向的值中减去 `b`。

```cpp
inline void write_sub(int32_t *a, int32_t b) 
```
这个函数定义了一个针对 `int32_t` 类型的原子减法操作，从指针 `a` 指向的值中减去 `b`。









## lgraph_date_time
Implements the DateTime, Date and TimeZone classes. 
// 实现了 DateTime，Date 和 TimeZone 类。

### namespace lgraph_api
// 命名空间 lgraph_api

### Functions
// 函数

```cpp
static inline constexpr int32_t MinDaysSinceEpochForDate()
```
min and max values that Date can hold
// Date 可以持有的最小值和最大值

```cpp
static inline constexpr int32_t MaxDaysSinceEpochForDate()
```
Maximum days since epoch for date
// Date 的最大纪元天数

```cpp
static inline constexpr int64_t MinMicroSecondsSinceEpochForDateTime()
```
min and max values that Date can hold
// DateTime 可以持有的最小值和最大值

```cpp
static inline constexpr int64_t MaxMicroSecondsSinceEpochForDateTime()
```
Maximum microseconds since epoch for date time
// DateTime 的最大纪元微秒数

### class Date
```cpp
#include <lgraph_date_time.h>
```
Implements the Date class. Range of dates is from 0/1/1 to 12/31/9999.
// 实现了 Date 类。日期范围为 0/1/1 到 12/31/9999。

#### Public Functions
// 公共函数

```cpp
Date()
```
Construct a new Date object with the date set to 1970/1/1.
// 构造一个新的 Date 对象，日期设置为 1970/1/1。

```cpp
explicit Date(const std::chrono::system_clock::time_point &tp)
```
Construct a new Date object with date set to the specified time. The time point must be in the range of 0/1/1 to 12/31/9999.
// 使用指定的时间构造一个新的 Date 对象。时间点必须在 0/1/1 到 12/31/9999 的范围内。

**抛出**  
OutOfRange – Thrown if the time point is out of range.
// **抛出** OutOfRange – 如果时间点超出范围则抛出。

**参数**  
tp – Time point to set the date to.
// **参数** tp – 要设置日期的时间点。

```cpp
explicit Date(const YearMonthDay &ymd)
```
Construct a new Date object with date set to the specified date. The date must be in the range of 0/1/1 to 12/31/9999.
// 使用指定的日期构造一个新的 Date 对象。日期必须在 0/1/1 到 12/31/9999 的范围内。

**抛出**  
OutOfRange – Thrown if the time point is out of range.
// **抛出** OutOfRange – 如果时间点超出范围则抛出。

**参数**  
ymd – Date in the form of year, month and day.
// **参数** ymd – 以年、月、日形式表示的日期。

```cpp
explicit Date(int32_t days_since_epoch)
```
Construct a new Date object with date set to an offset from epoch, i.e. the date is set to the specified number of days from epoch. The result date must be in the range of 0/1/1 to 12/31/9999.
// 构造一个新的 Date 对象，日期设置为相对于纪元的偏移，即日期设置为距离纪元的指定天数。结果日期必须在 0/1/1 到 12/31/9999 的范围内。

**抛出**  
OutOfRange – Thrown if the time point is out of range.
// **抛出** OutOfRange – 如果时间点超出范围则抛出。

**参数**  
days_since_epoch – Number of days since epoch.
// **参数** days_since_epoch – 从纪元开始的天数。

```cpp
explicit Date(const std::string &str)
```
Parse date from a YYYY-MM-DD string.
// 从 YYYY-MM-DD 字符串解析日期。

**抛出**  
InputError – if the string is not in the correct format.
// **抛出** InputError – 如果字符串不符合正确格式则抛出。

**参数**  
str – The string.
// **参数** str – 字符串。

```cpp
YearMonthDay GetYearMonthDay() const noexcept
```
Returns the current Date in the form of year, month and day.
// 返回当前日期的年、月、日形式。

**返回**  
The year month day.
// **返回** 年月日。

```cpp
Date operator+(int days) const
```
Add a number of days to the Date object.
// 向 Date 对象添加一定数量的天数。

**抛出**  
OutOfRange – Thrown if the time point is out of range.
// **抛出** OutOfRange – 如果时间点超出范围则抛出。

**参数**  
days – Number of days to add.
// **参数** days – 要添加的天数。

**返回**  
The resulting Date object.
// **返回** 结果 Date 对象。

```cpp
Date &operator+=(int days)
```
Add a number of days to the current Date object. In case of overflow, current object is not modified.
// 向当前 Date 对象添加一定数量的天数。在溢出的情况下，当前对象不进行修改。

**抛出**  
OutOfRange – Thrown if the resulting date is out of range.
// **抛出** OutOfRange – 如果结果日期超出范围则抛出。

**参数**  
days – Number of days to add.
// **参数** days – 要添加的天数。

**返回**  
Reference to the current date.
// **返回** 当前日期的引用。

```cpp
Date operator-(int days) const
```
Subtract a number of days from the Date object.
// 从 Date 对象中减去一定数量的天数。

**抛出**  
OutOfRange – Thrown if the resulting date is out of range.
// **抛出** OutOfRange – 如果结果日期超出范围则抛出。

**参数**  
days – Number of days to subtract.
// **参数** days – 要减去的天数。

**返回**  
The resulting Date object.
// **返回** 结果 Date 对象。

```cpp
Date &operator-=(int days)
```
Subtract a number of days from the current Date object. In case of overflow, current object is not modified.
// 从当前 Date 对象中减去一定数量的天数。在溢出的情况下，当前对象不进行修改。

**抛出**  
OutOfRange – Thrown if the resulting date is out of range.
// **抛出** OutOfRange – 如果结果日期超出范围则抛出。

**参数**  
days – Number of days to subtract.
// **参数** days – 要减去的天数。

**返回**  
Reference to the current Date object.
// **返回** 当前 Date 对象的引用。

```cpp
bool operator<(const Date &rhs) const noexcept
```
```cpp
bool operator<=(const Date &rhs) const noexcept
```
```cpp
bool operator>(const Date &rhs) const noexcept
```
```cpp
bool operator>=(const Date &rhs) const noexcept
```
```cpp
bool operator==(const Date &rhs) const noexcept
```
```cpp
bool operator!=(const Date &rhs) const noexcept
```
```cpp
int32_t DaysSinceEpoch() const noexcept
```
Returns the number of days this date is since epoch.
// 返回该日期距离纪元的天数。

```cpp
int32_t GetStorage() const noexcept
```
Returns the number of days this date is since epoch.
// 返回该日期距离纪元的天数。

```cpp
explicit operator int32_t() const noexcept
```
Returns the number of days this date is since epoch.
// 返回该日期距离纪元的天数。

```cpp
std::chrono::system_clock::time_point TimePoint() const noexcept
```
Returns the timepoint corresponding to this date at 00:00 am.
// 返回与该日期对应的时间点（00:00 AM）。

```cpp
std::string ToString() const noexcept
```
Get the string representation of the date in the format of YYYY-MM-DD.
// 获取日期的字符串表示，格式为 YYYY-MM-DD。

```cpp
explicit operator DateTime() const noexcept
```
Get the DateTime object corresponding to 00:00 am on this date.
// 获取与该日期的 00:00 AM 对应的 DateTime 对象。

#### Public Static Functions

```cpp
static bool Parse(const std::string &str, Date &d) noexcept
```
Parse date from YYYY-MM-DD, save value in d.  
从YYYY-MM-DD格式解析日期，并将结果保存到d中。

**参数**  
str – The string.  
str – 输入的字符串。

d – [out] The resulting Date.  
d – [输出] 解析结果的日期对象。

**返回**  
True if success, otherwise false.  
如果成功返回true，否则返回false。

```cpp
static size_t Parse(const char *beg, const char *end, Date &d) noexcept
```
Parse date from YYYY-MM-DD, save value in d.  
从YYYY-MM-DD格式解析日期，并将结果保存到d中。

**参数**  
beg – The beg.  
beg – 字符串的起始指针。

end – The end.  
end – 字符串的结束指针。

d – [out] The result.  
d – [输出] 解析结果的日期对象。

**返回**  
Number of bytes parsed (must be 10), 0 if failed.  
解析的字节数（必须为10），如果失败则返回0。

```cpp
static Date Now() noexcept
```
Returns the current Date.  
返回当前日期。

**返回**  
Current Date in UTC.  
以UTC格式返回当前日期。

```cpp
static Date LocalNow() noexcept
```
Returns the current Date in local timezone.  
返回本地时区的当前日期。

**返回**  
Current Date in local timezone.  
以本地时区格式返回当前日期。

#### Private Members

```cpp
int32_t days_since_epoch_
```
The days since epoch  
自纪元以来的天数。

### struct YearMonthDay
```cpp
#include <lgraph_date_time.h>
```
Structure representing a date in the format of year, month and day.  
用于表示日期的结构，包括年、月、日格式。

#### Public Members

```cpp
int year
```
The year, 0-9999  
年份，范围是0-9999。

```cpp
unsigned month
```
Month, 1-12  
月份，范围是1-12。

```cpp
unsigned day
```
Day, 1-31  
日期，范围是1-31。

### class DateTime
```cpp
#include <lgraph_date_time.h>
```
Implements a DateTime class that holds DateTime in the range of 0000-01-01 00:00:00.000000 to 9999-12-31 23:59:59.999999.  
实现一个DateTime类，范围从0000-01-01 00:00:00.000000到9999-12-31 23:59:59.999999。

#### Public Functions

```cpp
DateTime()
```
Construct a new DateTime object with date set to the epoch time, i.e., 1970-1-1 00:00:00.  
构造一个新的DateTime对象，日期设置为纪元时间，即1970-1-1 00:00:00。

```cpp
explicit DateTime(const std::chrono::system_clock::time_point &tp)
```
Construct a new DateTime object with date set to the specified timepoint.  
构造一个新的DateTime对象，日期设置为指定的时间点。

**抛出**  
OutOfRange – Thrown if the time point is out of range.  
超出范围 – 如果时间点超出范围则抛出。

**参数**  
tp – Timepoint to set the DateTime to.  
tp – 设置DateTime的时间点。

```cpp
explicit DateTime(const YMDHMSF &ymdhmsf)
```
Construct a new DateTime object with date set to the specified date and time given in YMDHMSF.  
构造一个新的DateTime对象，日期设置为在YMDHMSF中给定的指定日期和时间。

**抛出**  
OutOfRange – Thrown if the time point is out of range.  
超出范围 – 如果时间点超出范围则抛出。

**参数**  
ymdhmsf – Date and time to set the DateTime to.  
ymdhmsf – 设置DateTime的日期和时间。

```cpp
explicit DateTime(int64_t microseconds_since_epoch)
```
Construct a new DateTime object with date set to specified number of microseconds since epoch.  
构造一个新的DateTime对象，日期设置为从纪元以来指定的微秒数。

**抛出**  
OutOfRange – Thrown if the time point is out of range.  
超出范围 – 如果时间点超出范围则抛出。

**参数**  
microseconds_since_epoch – Number of microseconds since epoch.  
microseconds_since_epoch – 从纪元以来的微秒数。

```cpp
explicit DateTime(const std::string &str)
```
Construct a new DateTime object with date set to the specified date and time given in the form of YYYY-MM-DD HH:MM:SS[.FFFFFF].  
构造一个新的DateTime对象，日期设置为以YYYY-MM-DD HH:MM:SS[.FFFFFF]形式给定的指定日期和时间。

**抛出**  
OutOfRange – Thrown if the time point is out of range.  
超出范围 – 如果时间点超出范围则抛出。

InputError – Thrown if str has invalid format.  
输入错误 - 如果字符串格式无效则抛出。

**参数**  
str – String representation of the date and time in the form of YYYY-MM-DD HH:MM:SS.  
str – 日期和时间的字符串表示，格式为YYYY-MM-DD HH:MM:SS。

```cpp
YMDHMSF GetYMDHMSF() const noexcept
```
Get current DateTime in the form of year, month, day, hour, minute, second, fraction.  
以年、月、日、小时、分钟、秒和小数组成的形式获取当前DateTime。

**返回**  
The ymdhmsf.  
返回ymdhmsf格式的数据。

```cpp
DateTime operator+(int64_t n_microseconds) const
```
Add a number of microseconds to the DateTime.  
将一定数量的微秒添加到DateTime中。

**抛出**  
OutOfRange – Thrown if the resulting DateTime is out of range.  
超出范围 – 如果结果DateTime超出范围则抛出。

**参数**  
n_microseconds – Number of micorseconds to add.  
n_microseconds – 要添加的微秒数。

**返回**  
The resulting DateTime.  
结果DateTime。

```cpp
DateTime &operator+=(int64_t n_microseconds)
```
Adds a number of microseconds to the current DateTime object. In case of overflow, current object is not modified.  
将一定数量的微秒添加到当前DateTime对象。 如果发生溢出，当前对象不会被修改。

**抛出**  
OutOfRange – Thrown if the resulting DateTime is out of range.  
超出范围 – 如果结果DateTime超出范围则抛出。

**参数**  
n_microseconds – Number of microseconds to add.  
n_microseconds – 要添加的微秒数。

**返回**  
Reference to the current DateTime.  
返回对当前DateTime的引用。

```cpp
DateTime operator-(int64_t n_microseconds) const
```
Subtract a number of microseconds from the DateTime.  
从DateTime中减去一定数量的微秒。

**抛出**  
OutOfRange – Thrown if the resulting DateTime is out of range.  
超出范围 – 如果结果DateTime超出范围则抛出。

**参数**  
n_microseconds – Number of microseconds to subtract.  
n_microseconds – 要减去的微秒数。

**返回**  
The resulting DateTime.  
结果DateTime。

```cpp
DateTime &operator-=(int64_t n_microseconds)
```
Subtract a number of microseconds from the current DateTime object. In case of overflow, current object is not modified.  
从当前DateTime对象中减去一定数量的微秒。如果发生溢出，当前对象不会被修改。

**抛出**  
OutOfRange – Thrown if the resulting DateTime is out of range.  
超出范围 – 如果结果DateTime超出范围则抛出。

**参数**  
n_microseconds – Number of microseconds to subtract.  
n_microseconds – 要减去的微秒数。

**返回**  
Reference to the current DateTime.  
返回对当前DateTime的引用。

```cpp
bool operator<(const DateTime &rhs) const noexcept
```

```cpp
bool operator<=(const DateTime &rhs) const noexcept
```

```cpp
bool operator>(const DateTime &rhs) const noexcept
```

```cpp
bool operator>=(const DateTime &rhs) const noexcept
```

```cpp
bool operator==(const DateTime &rhs) const noexcept
```

```cpp
bool operator!=(const DateTime &rhs) const noexcept
```

```cpp
int64_t MicroSecondsSinceEpoch() const noexcept
```
Returns the number of microseconds since epoch.  
返回自纪元以来的微秒数。

```cpp
int64_t GetStorage() const noexcept
```
Returns the number of microseconds since epoch.  
返回自纪元以来的微秒数。

```cpp
explicit operator int64_t() const noexcept
```
Returns the number of microseconds since epoch.  
返回自纪元以来的微秒数。

```cpp
std::chrono::system_clock::time_point TimePoint() const noexcept
```
Returns the time point corresponding to this DateTime.  
返回与该DateTime对应的时间点。

```cpp
std::string ToString() const noexcept
```
Get the string representation of the DateTime in the format of YYYY-MM-DD HH:MM:SS[.FFFFFF].  
获得DateTime的字符串表示，格式为YYYY-MM-DD HH:MM:SS[.FFFFFF]。

```cpp
std::string ToDateString() const noexcept
```
Get the string representation of the date part of DateTime in the format of YYYY-MM-DD.  
获得DateTime日期部分的字符串表示，格式为YYYY-MM-DD。

```cpp
std::string ToTimeString() const noexcept
```
Get the string representation of the time part of DateTime in the format of HH:MM:SS[.FFFFFF].  
获得DateTime时间部分的字符串表示，格式为HH:MM:SS[.FFFFFF]。

#### Public Static Functions

```cpp
static bool Parse(const std::string &str, DateTime &dt) noexcept
```
Parse DateTime from string representation, save value in dt.  
从字符串表示中解析DateTime，并将值保存到dt中。

**参数**  
str – The string.  
str – 字符串。

dt – [out] The resulting DateTime.  
dt – [输出] 结果的DateTime。

**返回**  
True if success, otherwise false.  
成功返回true，否则返回false。

```cpp
static size_t Parse(const char *beg, const char *end, DateTime &dt) noexcept
```
Parse DateTime from string representation, save value in dt.  
从字符串表示中解析DateTime，并将值保存到dt中。

**参数**  
beg – The beg.  
beg – 开始指针。

end – The end.  
end – 结束指针。

dt – [out] The result.  
dt – [输出] 结果。

**返回**  
Number of bytes parsed (must be 26), 0 if failed.  
解析的字节数（必须为26），如果失败则返回0。

```cpp
static DateTime Now() noexcept
```
Returns the current DateTime.  
返回当前的DateTime。

**返回**  
Current DateTime in UTC.  
以UTC格式返回当前DateTime。

```cpp
static DateTime LocalNow() noexcept
```
Returns the current DateTime in local timezone.  
返回本地时区的当前DateTime。

**返回**  
Current DateTime in local timezone.  
以本地时区格式返回当前DateTime。

#### Private Members

```cpp
int64_t microseconds_since_epoch_
```
The microseconds since epoch.  
自纪元以来的微秒数。

### struct YMDHMSF
```cpp
#include <lgraph_date_time.h>
```
Structure representing a DateTime in the format of year, month, day, hour, minute, second and fraction.  
表示日期时间的结构，格式为年、月、日、小时、分钟、秒和分数。

#### Public Members

```cpp
int year
```
The year, 0-9999  
年份，范围是0到9999。

```cpp
unsigned month
```
Month, 1-12  
月份，范围是1到12。

```cpp
unsigned day
```
Day, 1-31  
天，范围是1到31。

```cpp
unsigned hour
```
Hour, 0-23  
小时，范围是0到23。

```cpp
unsigned minute
```
Minute, 0-59  
分钟，范围是0到59。

```cpp
unsigned second
```
Second, 0-59  
秒，范围是0到59。

```cpp
unsigned fraction
```
Fraction, 0-999999  
小数部分，范围是0到999999。

### class TimeZone
```cpp
#include <lgraph_date_time.h>
```
A class that represents a time zone.  
表示时区的类。

#### Public Functions

```cpp
explicit TimeZone(int time_diff_hours = 0)
```
Create a timezone which has time difference with UTC in hours `time_diff_hours`.  
创建与UTC有时差`time_diff_hours`小时的时区。

**抛出**  
InvalidParameter – Thrown if `time_diff_hours` is invalid.  
InvalidParameter – 如果`time_diff_hours`无效则抛出。

**参数**  
time_diff_hours – (Optional) Difference between local timezone and UTC. Must be >= -10 && <= 14. Otherwise, the function will throw.  
time_diff_hours – （可选）本地时区与UTC之间的差异。必须在-10到14之间。否则，该函数将抛出异常。

```cpp
DateTime FromUTC(const DateTime &dt) const
```
Convert a DateTime from UTC time to this time zone.  
将UTC时间的DateTime转换为此时区。

**抛出**  
OutOfRange – Thrown if the resulting value is out of range.  
OutOfRange – 如果结果值超出范围，则抛出。

**参数**  
dt – DateTime in UTC time.  
dt – UTC时间下的DateTime。

**返回**  
DateTime in local timezone.  
本地时区下的DateTime。

```cpp
DateTime ToUTC(const DateTime &dt) const
```
Convert a DateTime from this timezone to UTC time.  
将此时区的DateTime转换为UTC时间。

**抛出**  
OutOfRange – Thrown if the resulting value is out of range.  
OutOfRange – 如果结果值超出范围，则抛出。

**参数**  
dt – DateTime in local timezone.  
dt – 本地时区下的DateTime。

**返回**  
DateTime in UTC.  
UTC时间下的DateTime。

```cpp
int64_t UTCDiffSeconds() const noexcept
```
Returns diff from UTC in seconds, this is used in Date and DateTime.  
返回与UTC的秒数差，此差值用于日期和时间。

**返回**  
Number of seconds local timezone is from UTC.  
本地时区与UTC的秒数差。

```cpp
int64_t UTCDiffHours() const noexcept
```
Returns diff from UTC in hours, this is used in Date and DateTime.  
返回与UTC的小时数差，此差值用于日期和时间。

**返回**  
Number of hours local timezone is from UTC.  
本地时区与UTC的小时数差。

#### Public Static Functions

```cpp
static const TimeZone &LocalTimeZone() noexcept
```
Get local timezone.  
获取本地时区。

**返回**  
A const reference to local timezone.  
对本地时区的常量引用。

```cpp
static void UpdateLocalTimeZone() noexcept
```
Update local timezone, used only when daylight saving time changes. Daylight saving time may change after `LocalTZ` was initialized, in which case we need to update it. This function will update all references returned by `LocalTimeZone()`.  
更新本地时区，仅在夏令时变化时使用。夏令时可能在`LocalTZ`初始化后发生变化，在这种情况下，我们需要更新它。此函数将更新所有由`LocalTimeZone()`返回的引用。

#### Private Members

```cpp
int64_t time_diff_microseconds_
```
The difference in microseconds from UTC.  
与UTC的微秒差异。

#### Private Static Functions

```cpp
static TimeZone GetLocalTZ()
```
获取本地时区的私有静态函数。

```cpp
static TimeZone &LocalTZ()
```
获取本地时区的私有静态函数引用。











## lgraph_db

### namespace lgraph
### namespace lgraph_api

### Functions

#### bool ShouldKillThisTask()
Determine if we should kill current task.
确定是否应该终止当前任务。

**返回**  
True if a KillTask command was issued for this task, either due to user request or timeout.  
如果对该任务发出了KillTask命令（由于用户请求或超时），则返回True。

#### ThreadContextPtr GetThreadContext()
Gets thread context pointer, which can then be used in ShouldKillThisTask(ctx). Calling ShouldKillThisTask() is equivalent to ShouldKillThisTask(GetThreadContext()). In order to save the cost of GetThreadContext(), you can store the context and use it in ShouldKillThisTask(ctx).
获取线程上下文指针，然后可以在ShouldKillThisTask(ctx)中使用。调用ShouldKillThisTask()等效于调用ShouldKillThisTask(GetThreadContext())。为了节省GetThreadContext()的开销，可以存储上下文并在ShouldKillThisTask(ctx)中使用。

**Example:** 
```c++
ThreadContextPtr ctx = GetThreadContext(); 
while (HasMoreWorkToDo()) { 
    if (ShouldKillThisTask(ctx)) { 
        break; 
    } 
    DoWork(); 
}
```

**返回**  
The thread context.  
线程上下文。

#### bool ShouldKillThisTask(ThreadContextPtr ctx)
Determine if we should kill the task currently running in the thread identified by ctx.
确定是否应该终止在ctx标识的线程中当前运行的任务。

**参数**  
- ctx – The context, as obtained with GetThreadContext.  
参数ctx – 通过GetThreadContext获得的上下文。

**返回**  
True if a KillTask command was issued for this task.  
如果对该任务发出了KillTask命令，则返回True。

### Variables

#### const typedef void * ThreadContextPtr
Defines an alias representing the thread context pointer.  
定义代表线程上下文指针的别名。

### class GraphDB
```cpp
#include <lgraph_db.h>
```
GraphDB represents a graph instance. In TuGraph, each graph instance has its own schema and access control settings. Accessing a GraphDB without appropriate access rights yields WriteNotAllowed.  
GraphDB表示一个图实例。在TuGraph中，每个图实例都有自己的模式和访问控制设置。没有适当访问权限访问GraphDB会导致WriteNotAllowed。

A GraphDB becomes invalid if Close() is called, in which case all transactions and iterators associated with that GraphDB become invalid. Further operation on that GraphDB yields InvalidGraphDB.  
如果调用Close()，则GraphDB变为无效，此时与该GraphDB相关的所有事务和迭代器都会变得无效。对该GraphDB的进一步操作将导致InvalidGraphDB。

### Public Functions

#### explicit GraphDB(lgraph::AccessControlledDB *db_with_access_control, bool read_only, bool owns_db = false)
For internal use only. Users should use Galaxy::OpenGraph() to get GraphDB.  
仅供内部使用。用户应使用Galaxy::OpenGraph()获取GraphDB。

#### GraphDB(GraphDB&&)
#### GraphDB &operator=(GraphDB&&)
#### ~GraphDB()
#### void Close()
Close the graph. This will close the graph and release all transactions, iterators associated with the graph. After calling Close(), the graph becomes invalid, and cannot be used anymore.  
关闭图。此操作将关闭图并释放与图相关的所有事务和迭代器。调用Close()后，图将变为无效，无法再使用。

#### Transaction CreateReadTxn()
Creates a read transaction.  
创建一个读事务。

**抛出**  
InvalidGraphDB – Thrown when currently GraphDB is invalid.  
InvalidGraphDB – 当当前GraphDB无效时抛出。

**返回**  
The new read transaction.  
新的读事务。

#### Transaction CreateWriteTxn(bool optimistic = false)
Creates a write transaction. Write operations can only be performed in write transactions, otherwise exceptions will be thrown. A write transaction can be optimistic. Optimistic transactions can run in parallel and any conflict will be detected during commit. If a transaction conflicts with an earlier one, a TxnConflict will be thrown during commit.  
创建一个写事务。写操作仅能在写事务中执行，否则会抛出异常。写事务可以是乐观的。乐观事务可以并行运行，任何冲突将在提交时被检测到。如果事务与先前事务冲突，提交时将抛出TxnConflict。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。

**参数**  
- optimistic – (Optional) True to create an optimistic transaction.  
- optimistic –（可选）True以创建乐观事务。

**返回**  
The new write transaction.  
新的写事务。

#### Transaction ForkTxn(Transaction &txn)
Forks a read transaction. The resulting read transaction will share the same view as the forked one, meaning that when reads are performed on the same vertex/edge, the results will always be identical, whether they are performed in the original transaction or the forked one. Only read transactions can be forked. Calling ForkTxn() on a write txn causes an InvalidFork to be thrown.  
分叉一个读事务。结果读事务将与被分叉的事务共享相同的视图，这意味着当在同一个顶点/边上执行读取时，无论是在原始事务中还是分叉事务中执行，结果总是相同的。只有读事务可以被分叉。在写事务上调用ForkTxn()会导致抛出InvalidFork。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- InvalidFork – Thrown when txn is a write transaction.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- InvalidFork – 当txn是一个写事务时抛出。

**参数**  
- txn – [in] The read transaction to be forked.  
- txn – [in] 要分叉的读事务。

**返回**  
A new read Transaction that shares the same view with txn.  
一个新的读事务，与txn共享相同的视图。

#### void Flush()
Flushes buffered data to disk. If there have been some async transactions, there could be data that are written to this graph, but not persisted to disk yet. Calling Flush() will persist the data and prevent data loss in case of system crash.  
将缓冲数据刷新到磁盘。如果存在一些异步事务，可能有数据已写入此图，但尚未持久化到磁盘。调用Flush()将持久化数据，并防止在系统崩溃时丢失数据。

**抛出**  
InvalidGraphDB – Thrown when currently GraphDB is invalid.  
InvalidGraphDB – 当前GraphDB无效时抛出。

#### void DropAllData()
Drop all the data in the graph, including labels, indexes and vertexes/edges.  
删除图中的所有数据，包括标签、索引和顶点/边。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。

#### void DropAllVertex()
Drop all vertex and edges but keep the labels and indexes.  
删除所有顶点和边，但保留标签和索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。

#### size_t EstimateNumVertices()
Estimate number of vertices. We don’t maintain the exact number of vertices, but only the next vid. This function actually returns the next vid to be used. So if you have deleted a lot of vertices, the result can be quite different from actual number of vertices.  
估算顶点数量。我们并不维护确切的顶点数量，而只是维护下一个vid。此函数实际上返回即将使用的下一个vid。因此，如果您删除了很多顶点，则结果可能与实际顶点数量差异很大。

**抛出**  
InvalidGraphDB – Thrown when currently GraphDB is invalid.  
InvalidGraphDB – 当前GraphDB无效时抛出。

**返回**  
Estimated number of vertices.  
估计的顶点数量。

#### bool AddVertexLabel(const std::string &label, const std::vector<FieldSpec> &fds, const VertexOptions &options)
Adds a vertex label.  
添加一个顶点标签。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InputError – Thrown if the schema is illegal.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果模式不合法，则抛出。

**参数**  
- label – The label name.  
- fds – The field specifications.  
- primary_field – The primary field.  
- label – 标签名称。  
- fds – 字段规范。  
- primary_field – 主字段。

**返回**  
True if it succeeds, false if the label already exists.  
如果成功则返回True，如果标签已存在则返回false。

#### bool DeleteVertexLabel(const std::string &label, size_t *n_modified = nullptr)
Deletes a vertex label and all the vertices with this label.  
删除一个顶点标签及其所有顶点。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。

**参数**  
- label – The label name.  
- n_modified – [out] (Optional) If non-null, return the number of deleted vertices.  
- label – 标签名称。  
- n_modified – [out]（可选）如果不为null，返回删除的顶点数量。

**返回**  
True if it succeeds, false if the label does not exist.  
如果成功则返回True，如果标签不存在则返回false。

#### bool AlterVertexLabelDelFields(const std::string &label, const std::vector<std::string> &del_fields, size_t *n_modified = nullptr)
Deletes fields in a vertex label. This function also updates the vertex data and indices accordingly to make sure the database remains in a consistent state.  
删除顶点标签中的字段。此函数还会相应地更新顶点数据和索引，以确保数据库保持一致的状态。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InputError – Thrown if field not found, or some fields cannot be deleted.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果未找到字段，或者某些字段无法删除，则抛出。

**参数**  
- label – The label name.  
- del_fields – Labels of the fields to be deleted.  
- n_modified – [out] (Optional) If non-null, return the number of modified vertices.  
- label – 标签名称。  
- del_fields – 要删除字段的标签。  
- n_modified – [out]（可选）如果不为null，返回修改后的顶点数量。

**返回**  
True if it succeeds, false if the label does not exist.  
如果成功则返回True，如果标签不存在则返回false。

#### bool AlterVertexLabelAddFields(const std::string &label, const std::vector<FieldSpec> &add_fields, const std::vector<FieldData> &default_values, size_t *n_modified = nullptr)
Add fields to a vertex label. The new fields in existing vertices will be filled with default values.  
向顶点标签添加字段。现有顶点中的新字段将填充默认值。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.  
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.  
- InputError – Thrown if field already exists.  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在具有只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果字段已存在，则抛出。

**参数**  
- label – The label name.  
- add_fields – The add fields.  
- default_values – The default values of the newly added fields.  
- n_modified – [out] (Optional) If non-null, return the number of modified vertices.  
- label – 标签名称。  
- add_fields – 添加的字段。  
- default_values – 新添加字段的默认值。  
- n_modified – [out]（可选）如果不为null，返回修改后的顶点数量。

**返回**  
True if it succeeds, false if the label does not exist.  
如果成功则返回True，如果标签不存在则返回false。

#### bool AlterVertexLabelModFields(const std::string &label, const std::vector<FieldSpec> &mod_fields, size_t *n_modified = nullptr)
Modify fields in a vertex label, either change the data type or optional, or both.
// 修改顶点标签中的字段，可以改变数据类型、选项，或两者均可。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if field not found, or is of incompatible data type.
// 如果找不到字段或者字段的数据类型不兼容，则抛出。

**参数**  
- label – The label name.
// 标签名称。
- mod_fields – The new specification of the modified fields.
// 修改字段的新规格。
- n_modified – [out] (Optional) If non-null, return the number of modified vertices.
// [输出]（可选）如果非空，返回修改的顶点数量。

**返回**  
True if it succeeds, false if the label does not exist.
// 如果成功返回真，如果标签不存在则返回假。

#### bool AddEdgeLabel(const std::string &label, const std::vector<FieldSpec> &fds, const EdgeOptions &options)
Add an edge label, specifying its schema. It is allowed to specify edge constraints, too. An edge can be bound to several (source_label, destination_label) pairs, which makes sure this type of edges will only be added between these types of vertices. By default, the constraint is empty, meaning that the edge is not restricted.
// 添加边标签，指定其模式。也允许指定边约束。一个边可以绑定到多个 (source_label, destination_label) 对，这确保这种类型的边只会在这些类型的顶点之间添加。默认情况下，约束为空，意味着边没有限制。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – if invalid schema (invalid specification, re-definition of the same field, etc.).
// 如果模式无效（规格无效、重新定义相同字段等）则抛出。

**参数**  
- label – The label name.
// 标签名称。
- fds – The field specifications.
// 字段规格。
- options – The edge options.
// 边选项。

**返回**  
True if it succeeds, false if the label already exists.
// 如果成功返回真，如果标签已经存在则返回假。

#### bool DeleteEdgeLabel(const std::string &label, size_t *n_modified = nullptr)
Deletes an edge label and all the edges with this label.
// 删除边标签及其所有相关的边。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。

**参数**  
- label – The label name.
// 标签名称。
- n_modified – [out] (Optional) If non-null, return the number of deleted edges.
// [输出]（可选）如果非空，返回已删除边的数量。

**返回**  
True if it succeeds, false if the label does not exist.
// 如果成功返回真，如果标签不存在则返回假。

#### bool AlterEdgeLabelDelFields(const std::string &label, const std::vector<std::string> &del_fields, size_t *n_modified = nullptr)
Deletes fields in an edge label. This function also updates the edge data and indices accordingly to make sure the database remains in a consistent state.
// 删除边标签中的字段。此函数还会相应地更新边数据和索引，以确保数据库保持一致的状态。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if field not found, or some fields cannot be deleted.
// 如果找不到字段，或者某些字段无法被删除，则抛出。

**参数**  
- label – The label name.
// 标签名称。
- del_fields – Labels of the fields to be deleted.
// 要删除的字段标签。
- n_modified – [out] (Optional) If non-null, return the number of modified edges.
// [输出]（可选）如果非空，返回修改的边数量。

**返回**  
True if it succeeds, false if the label does not exist.
// 如果成功返回真，如果标签不存在则返回假。

#### bool AlterEdgeLabelAddFields(const std::string &label, const std::vector<FieldSpec> &add_fields, const std::vector<FieldData> &default_values, size_t *n_modified = nullptr)
Add fields to an edge label. The new fields in existing edges will be filled with default values.
// 向边标签添加字段。现有边中的新字段将填充默认值。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if field already exists.
// 如果字段已经存在则抛出。

**参数**  
- label – The label name.
// 标签名称。
- add_fields – The add fields.
// 添加的字段。
- default_values – The default values of the newly added fields.
// 新添加字段的默认值。
- n_modified – [out] (Optional) If non-null, return the number of modified edges.
// [输出]（可选）如果非空，返回修改的边数量。

**返回**  
True if it succeeds, false if the label does not exist.
// 如果成功返回真，如果标签不存在则返回假。

#### bool AlterEdgeLabelModFields(const std::string &label, const std::vector<FieldSpec> &mod_fields, size_t *n_modified = nullptr)
Modify fields in an edge label, either change the data type or optional, or both.
// 修改边标签中的字段，可以改变数据类型、选项，或两者均可。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if field not found, or is of incompatible data type.
// 如果找不到字段或者字段的数据类型不兼容，则抛出。

**参数**  
- label – The label name.
// 标签名称。
- mod_fields – The new specification of the modified fields.
// 修改字段的新规格。
- n_modified – [out] (Optional) If non-null, return the number of modified edges.
// [输出]（可选）如果非空，返回修改的边数量。

**返回**  
True if it succeeds, false if the label does not exist.
// 如果成功返回真，如果标签不存在则返回假。


#### bool AddVertexIndex(const std::string &label, const std::string &field, IndexType type)
Adds an index to ‘label:field’. This function blocks until the index is fully created.
// 为 'label:field' 添加索引。此函数阻止执行直到索引完全创建。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label:field does not exist, or not indexable.
// 如果 label:field 不存在或者不可索引，则抛出。

**参数**  
- label – The label name.
// 标签名称。
- field – The field name.
// 字段名称。
- is_unique – True if it's a unique index, false if not.
// 如果是唯一索引则为真，不是则为假。

**返回**  
True if it succeeds, false if the index already exists.
// 如果成功返回真，如果索引已经存在则返回假。

---

#### bool AddEdgeIndex(const std::string &label, const std::string &field, IndexType type)
Adds an index to ‘label:field’. This function blocks until the index is fully created.
// 为 'label:field' 添加索引。此函数阻止执行直到索引完全创建。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label:field does not exist, or not indexable.
// 如果 label:field 不存在或者不可索引，则抛出。

**参数**  
- label – The label.
// 标签。
- field – The field.
// 字段。
- is_unique – True if the field content is unique for each vertex.
// 如果字段内容对每个顶点都是唯一的，则为真。

**返回**  
True if it succeeds, false if the index already exists.
// 如果成功返回真，如果索引已经存在则返回假。

---

#### bool AddVertexCompositeIndex(const std::string &label, const std::vector<std::string> &fields, CompositeIndexType type)

#### bool AddVectorIndex(bool is_vertex, const std::string &label, const std::string &field, const std::string &index_type, int vec_dimension, const std::string &distance_type, std::vector<int> &index_spec)
Adds a vector index to ‘label:field’. This function blocks until the index is fully created.
// 为 'label:field' 添加向量索引。此函数阻止执行直到索引完全创建。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label:field does not exist, or not indexable.
// 如果 label:field 不存在或者不可索引，则抛出。

**参数**  
- is_vertex – Vertex or edge.
// 顶点或边。
- label – The label.
// 标签。
- field – The field.
// 字段。
- is_unique – True if the field content is unique for each vertex.
// 如果字段内容对每个顶点都是唯一的，则为真。
- index_type – Type of the index.
// 索引类型。
- vec_dimension – Dimension of the vector.
// 向量的维度。
- distance_type – Type of the distance.
// 距离的类型。
- index_spec – Specification of the index.
// 索引的规格。

**返回**  
True if it succeeds, false if the index already exists.
// 如果成功返回真，如果索引已经存在则返回假。

---

#### bool IsVertexIndexed(const std::string &label, const std::string &field)
Check if this vertex_label:field is indexed.
// 检查这个 vertex_label:field 是否被索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- InputError – Thrown if label:field does not exist.
// 如果 label:field 不存在则抛出。

**参数**  
- label – The label.
// 标签。
- field – The field.
// 字段。

**返回**  
True if index exists, false if label:field exists but not indexed.
// 如果索引存在则返回真，如果 label:field 存在但没有被索引则返回假。

---

#### bool IsEdgeIndexed(const std::string &label, const std::string &field)
Check if this edge_label:field is indexed.
// 检查这个 edge_label:field 是否被索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- InputError – Thrown if label:field does not exist.
// 如果 label:field 不存在则抛出。

**参数**  
- label – The label.
// 标签。
- field – The field.
// 字段。

**返回**  
True if index exists, false if label:field exists but not indexed.
// 如果索引存在则返回真，如果 label:field 存在但没有被索引则返回假。

---

#### bool IsVertexCompositeIndexed(const std::string &label, const std::vector<std::string> &field)

#### bool DeleteVertexIndex(const std::string &label, const std::string &field)
Deletes the index to ‘vertex_label:field’.
// 删除对 'vertex_label:field' 的索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label or field does not exist.
// 如果标签或字段不存在，则抛出。

**参数**  
- label – The label.
// 标签。
- field – The field.
// 字段。

**返回**  
True if it succeeds, false if the index does not exist.
// 如果成功返回真，如果索引不存在则返回假。

---

#### bool DeleteVertexCompositeIndex(const std::string &label, const std::vector<std::string> &fields)

#### bool DeleteEdgeIndex(const std::string &label, const std::string &field)
Deletes the index to ‘edge_label:field’.
// 删除对 'edge_label:field' 的索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label or field does not exist.
// 如果标签或字段不存在，则抛出。

**参数**  
- label – The label.
// 标签。
- field – The field.
// 字段。

**返回**  
True if it succeeds, false if the index does not exist.
// 如果成功返回真，如果索引不存在则返回假。

---

#### bool DeleteVectorIndex(bool is_vertex, const std::string &label, const std::string &field)
Deletes the vector index to ‘vertex_label:field’.
// 删除对 'vertex_label:field' 的向量索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
// 当当前 GraphDB 无效时抛出。
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
// 当在只读访问级别的 GraphDB 上调用时抛出。
- InputError – Thrown if label or field does not exist.
// 如果标签或字段不存在，则抛出。

**参数**  
- is_vertex – Vertex or edge.
// 顶点或边。
- label – The label.
// 标签。
- field – The field.
// 字段。

**返回**  
True if it succeeds, false if the index does not exist.
// 如果成功返回真，如果索引不存在则返回假。

---

#### std::string GetDescription() const
Get graph description.
获取图形描述。

**抛出**  
InvalidGraphDB – Thrown when currently GraphDB is invalid.
抛出异常：InvalidGraphDB – 当前GraphDB无效时抛出。

**返回**  
The description.
返回图形描述。

---

#### size_t GetMaxSize() const
Get maximum graph size.
获取最大图形大小。

**抛出**  
InvalidGraphDB – Thrown when currently GraphDB is invalid.
抛出异常：InvalidGraphDB – 当前GraphDB无效时抛出。

**返回**  
The maximum size.
返回最大大小。

---

#### bool AddVertexFullTextIndex(const std::string &vertex_label, const std::string &field)
Add fulltext index to ‘vertex_label:field’.
将全文索引添加到‘vertex_label:field’。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
- InputError – Thrown if vertex label or field does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果顶点标签或字段不存在，则抛出。  

**参数**  
- vertex_label – The vertex label.
顶点标签。

- field – The field.
字段。

**返回**  
True if it succeeds, false if the fulltext index already exists.
如果成功返回true，如果全文索引已存在则返回false。

---

#### bool AddEdgeFullTextIndex(const std::string &edge_label, const std::string &field)
Add fulltext index to ‘edge_label:field’.
将全文索引添加到‘edge_label:field’。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
- InputError – Thrown if edge label or field does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果边标签或字段不存在，则抛出。  

**参数**  
- edge_label – The edge label.
边标签。

- field – The field.
字段。

**返回**  
True if it succeeds, false if the fulltext index already exists.
如果成功返回true，如果全文索引已存在则返回false。

---

#### bool DeleteVertexFullTextIndex(const std::string &vertex_label, const std::string &field)
Delete the fulltext index of ‘vertex_label:field’.
删除‘vertex_label:field’的全文索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
- InputError – Thrown if vertex label or field does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果顶点标签或字段不存在，则抛出。  

**参数**  
- vertex_label – The vertex label.
顶点标签。

- field – The field.
字段。

**返回**  
True if it succeeds, false if the fulltext index does not exist.
如果成功返回true，如果全文索引不存在则返回false。

---

#### bool DeleteEdgeFullTextIndex(const std::string &edge_label, const std::string &field)
Delete the fulltext index of ‘edge_label:field’.
删除‘edge_label:field’的全文索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.
- InputError – Thrown if edge label or field does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  
- InputError – 如果边标签或字段不存在，则抛出。  

**参数**  
- edge_label – The edge label.
边标签。

- field – The field.
字段。

**返回**  
True if it succeeds, false if the fulltext index does not exist.
如果成功返回true，如果全文索引不存在则返回false。

---

#### void RebuildFullTextIndex(const std::set<std::string> &vertex_labels, const std::set<std::string> &edge_labels)
Rebuild the fulltext index of vertex_labels and edge_labels.
重建顶点标签和边标签的全文索引。

**参数**  
- vertex_labels – The vertex labels whose fulltext index need to be rebuilt.
需要重建全文索引的顶点标签。

- edge_labels – The edge labels whose fulltext index need to be rebuilt.
需要重建全文索引的边标签。

---

#### std::vector<std::tuple<bool, std::string, std::string>> ListFullTextIndexes()
List fulltext indexes of vertex and edge.
列出顶点和边的全文索引。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  

**返回**  
Format of returned data: (is_vertex, label_name, property_name).
返回数据格式：(is_vertex, label_name, property_name)。

---

#### std::vector<std::pair<int64_t, float>> QueryVertexByFullTextIndex(const std::string &label, const std::string &query, int top_n)
Query vertex by fulltext index.
通过全文索引查询顶点。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- InputError – Thrown if label does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- InputError – 如果标签不存在，则抛出。  

**参数**  
- label – The vertex label.
标签 – 顶点标签。

- query – Lucene query language.
查询 – Lucene查询语言。

- top_n – Return top n data.
top_n – 返回前n条数据。

**返回**  
Vertex vids and score. Throws exception on error.
返回顶点的vid和分数。发生错误时抛出异常。

---

#### std::vector<std::pair<EdgeUid, float>> QueryEdgeByFullTextIndex(const std::string &label, const std::string &query, int top_n)
Query edge by fulltext index.
通过全文索引查询边。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- InputError – Thrown if label does not exist.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- InputError – 如果标签不存在，则抛出。  

**参数**  
- label – The edge label.
标签 – 边标签。

- query – Lucene query language.
查询 – Lucene查询语言。

- top_n – Return top n data.
top_n – 返回前n条数据。

**返回**  
Edge uids and score. Throws exception on error.
返回边的uids和分数。发生错误时抛出异常。

---

#### void RefreshCount()
Recount the total number of vertex and edge, stop writing during the count.
重新计算顶点和边的总数，在计数期间停止写入。

**抛出**  
- InvalidGraphDB – Thrown when currently GraphDB is invalid.
- WriteNotAllowed – Thrown when called on a GraphDB with read-only access level.

抛出异常：  
- InvalidGraphDB – 当前GraphDB无效时抛出。  
- WriteNotAllowed – 在只读访问级别的GraphDB上调用时抛出。  

---

### Private Functions

#### GraphDB(const GraphDB&) = delete
Copying is disabled.
复制功能被禁止。

#### GraphDB &operator=(const GraphDB&) = delete
赋值操作被禁止。

### Private Members

#### lgraph::AccessControlledDB *db_
数据库指针。

#### bool should_delete_db_ = false
是否应该删除数据库的标志，默认值为false。

#### bool read_only_ = false
是否只读的标志，默认值为false。










## lgraph_edge_index_iterator

```cpp
namespace lgraph {
namespace lgraph_api {
class EdgeIndexIterator {
#include <lgraph_edge_index_iterator.h>
```

`EdgeIndexIterator` can be used to access a set of edges that has the same indexed value. 如果索引是唯一的（即每条边具有唯一的索引值），那么每个 `EdgeIndexIterator` 只会有一个边的唯一 ID，并且在调用 `Next()` 后将变得无效。

An `EdgeIndexIterator` is valid iff it points to a valid `(index_value, euid)` pair; otherwise, it is invalid. `EdgeIndexIterator` 在指向有效的 `(index_value, euid)` 对时有效；否则，它是无效的。调用无效 `EdgeIndexIterator` 的成员函数会抛出异常，唯一不抛出异常的函数是 `IsValid()`。

### Public Functions

- **EdgeIndexIterator(EdgeIndexIterator &&rhs)**

- **EdgeIndexIterator &operator=(EdgeIndexIterator&&)**

- **~EdgeIndexIterator()**

- **void Close()**
  - Closes this iterator.
  - 关闭此迭代器。

- **bool IsValid() const**
  - Query if this iterator is valid, i.e., the Key and Vid can be queried.
  - 查询此迭代器是否有效，即 Key 和 Vid 是否可以查询。
  
  **返回**
  - True if valid, false if not.
  - 如果有效返回 True，如果无效返回 false。

- **bool Next()**
  - Move to the next edge unique id in the list, which consists of all the valid edge unique ids of the iterator and is sorted from small to large. If we hit the end of the list, the iterator will become invalid and false is returned.
  - 移动到列表中的下一个边的唯一 ID，该列表由迭代器的所有有效边的唯一 ID 组成，并按从小到大的顺序排序。如果到达列表的末尾，则迭代器将变为无效并返回 false。
  
  **返回**
  - True if it succeeds, otherwise false.
  - 如果成功返回 True，否则返回 false。

- **FieldData GetIndexValue() const**
  - Gets the current index value. The euids are sorted in `(EdgeIndexValue, euid)` order. When `Next()` is called, the iterator moves from one euid to the next, possibly moving from one `EdgeIndexValue` to another. This function tells the `EdgeIndexValue` currently pointed to.
  - 获取当前的索引值。euids 按 `(EdgeIndexValue, euid)` 顺序排序。当调用 `Next()` 时，迭代器从一个 euid 移动到下一个，可能会从一个 `EdgeIndexValue` 移动到另一个。此函数返回当前指向的 `EdgeIndexValue`。
  
  **返回**
  - The key.
  - 返回键值。

- **EdgeUid GetUid() const**
  - Gets the Edge Unique Id.
  - 获取边的唯一 ID。
  
  **返回**
  - The UID.
  - 返回 UID。

- **int64_t GetSrc() const**
  - Gets the source vertex id.
  - 获取源顶点 ID。
  
  **返回**
  - The source vertex id.
  - 返回源顶点 ID。

- **int64_t GetDst() const**
  - Gets the destination of the edge.
  - 获取边的目的地。
  
  **返回**
  - The destination vertex id.
  - 返回目的顶点 ID。

- **size_t GetLabelId() const**
  - Gets label id of this edge.
  - 获取此边的标签 ID。
  
  **返回**
  - The label identifier.
  - 返回标签标识符。

- **int64_t GetEdgeId() const**
  - Gets edge id.
  - 获取边的 ID。
  
  **返回**
  - The edge identifier.
  - 返回边标识符。

### Private Functions

- **EdgeIndexIterator(lgraph::EdgeIndexIterator &&it, const std::shared_ptr<lgraph::Transaction> &txn)**

- **EdgeIndexIterator(const EdgeIndexIterator&) = delete**

- **EdgeIndexIterator &operator=(const EdgeIndexIterator&) = delete**

### Private Members

- **std::unique_ptr<lgraph::EdgeIndexIterator> it_**

- **std::shared_ptr<lgraph::Transaction> txn_**

### Friends

- **friend class Transaction**














## lgraph_edge_iterator

### namespace lgraph
### namespace graph
### namespace lgraph_api

### Functions

```cpp
bool operator==(const OutEdgeIterator &lhs, const OutEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator==(const OutEdgeIterator &lhs, const InEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator==(const InEdgeIterator &lhs, const OutEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator==(const InEdgeIterator &lhs, const InEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator!=(const OutEdgeIterator &lhs, const OutEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator!=(const OutEdgeIterator &lhs, const InEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator!=(const InEdgeIterator &lhs, const OutEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

```cpp
bool operator!=(const InEdgeIterator &lhs, const InEdgeIterator &rhs)
```
Check whether lhs and rhs points to the same edge.  // 检查 lhs 和 rhs 是否指向同一条边。

### class InEdgeIterator
```cpp
#include <lgraph_edge_iterator.h>
```
An InEdgeIterator can be used to iterate through the in-coming edges of a vertex. Edges are sorted in (lid, tid, src, eid) order, and each (dst, lid, tid, src, eid) tuple is guaranteed to uniquely identify an edge.  // InEdgeIterator 可用于遍历一个顶点的入边。边按 (lid, tid, src, eid) 的顺序排序，每个 (dst, lid, tid, src, eid) 元组保证唯一标识一条边。

An InEdgeIterator is valid iff it points to a valid in-coming edge; otherwise, it is invalid. Calling member function on an invalid InEdgeIterator throws an exception, except for the IsValid() and Goto() functions.  // 当 InEdgeIterator 指向一个有效的入边时，它是有效的；否则，它是无效的。在无效的 InEdgeIterator 上调用成员函数会抛出异常，除了 IsValid() 和 Goto() 函数。

The following operations invalidate an InEdgeIterator:  // 以下操作会使 InEdgeIterator 无效：

- Constructing an InEdgeIterator for non-existing edge.  // 为不存在的边构造 InEdgeIterator。
- Calling Goto() with the id of a non-existing edge.  // 使用不存在的边的 ID 调用 Goto()。
- Calling Next() on the last in-coming edge.  // 在最后一个入边上调用 Next()。
- Calling Delete() on the last in-coming edge.  // 在最后一个入边上调用 Delete()。
- Calling Close() on the iterator.  // 在迭代器上调用 Close()。

In TuGraph, every iterator belongs to a transaction, and can only be used when the transaction is valid. Calling member functions on an iterator inside an invalid transaction yields InvalidTxn, except for Invalid().  // 在 TuGraph 中，每个迭代器属于一个事务，只能在事务有效时使用。在无效事务中的迭代器上调用成员函数会返回 InvalidTxn，Invalid() 除外。

#### Public Functions

```cpp
InEdgeIterator(InEdgeIterator &&rhs)
```

```cpp
InEdgeIterator &operator=(InEdgeIterator&&)
```

```cpp
~InEdgeIterator()
```

```cpp
void Close() noexcept
```
Closes this iterator. The iterator turns invalid after being closed.  // 关闭此迭代器。关闭后，迭代器变为无效。

```cpp
bool Next()
```
Move to the next incoming edge to current destination vertex. If there is no more edge, the iterator becomes invalid and false is returned.  // 移动到当前目标顶点的下一个入边。如果没有更多边，迭代器变为无效并返回 false。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
True if it succeeds, false if it fails.  // 如果成功则返回 true，失败则返回 false。

```cpp
bool Goto(EdgeUid euid, bool nearest = false)
```
Go to the edge specified by euid. If the specified edge cannot be found and nearest==true, then try to get the next in-coming edge to the vertex euid.dst, sorted by (label, tid, src, eid). If there is no such edge, iterator is invalidated and false is returned.  // 转到由 euid 指定的边。如果无法找到指定的边且 nearest==true，则尝试获取到顶点 euid.dst 的下一个入边，按 (label, tid, src, eid) 排序。如果没有这样的边，迭代器变为无效并返回 false。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**参数**
- euid – Edge Unique Id.  // 边的唯一 ID。
- nearest – (Optional) True to get the nearest edge if the specified one cannot be found.  // （可选）如果未找到指定的边，则为真以获取最近的边。

**返回**
True if it succeeds, false if it fails.  // 如果成功则返回 true，失败则返回 false。

```cpp
EdgeUid GetUid() const
```
Gets the Edge Unique Id.  // 获取边的唯一 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The UID.  // 返回 UID。

```cpp
int64_t GetSrc() const
```
Gets the source vertex id.  // 获取源顶点 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The source vertex id.  // 返回源顶点 ID。

```cpp
int64_t GetDst() const
```
Gets destination vertex id.  // 获取目标顶点 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The destination vertex id.  // 返回目标顶点 ID。

```cpp
int64_t GetEdgeId() const
```
Gets edge id.  // 获取边 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The edge id.  // 返回边 ID。

```cpp
int64_t GetTemporalId() const
```
Gets temporal id.  // 获取时间 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The temporal id.  // 返回时间 ID。

```cpp
bool IsValid() const
```
Query if this iterator is valid.  // 查询此迭代器是否有效。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
True if valid, false if not.  // 如果有效则返回 true，如果无效则返回 false。

```cpp
const std::string &GetLabel() const
```
Gets the label of this edge.  // 获取该边的标签。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The label.  // 返回标签。

```cpp
int16_t GetLabelId() const
```
Gets label id of this edge.  // 获取该边的标签 ID。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

**返回**
The label identifier.  // 返回标签标识符。

```cpp
std::vector<FieldData> GetFields(const std::vector<std::string> &field_names) const
```
Gets the fields specified.  // 获取指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if any field does not exist.  // 如果任何字段不存在，则抛出 InputError。

**参数**
field_names – List of names of the fields.  // 字段名称的列表。

**返回**
The fields.  // 返回字段。

```cpp
FieldData GetField(const std::string &field_name) const
```
Gets the field specified.  // 获取指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if field does not exist.  // 如果字段不存在，则抛出 InputError。

**参数**
field_name – Field name.  // 字段名称。

**返回**
Field value.  // 返回字段值。

```cpp
std::vector<FieldData> GetFields(const std::vector<size_t> &field_ids) const
```
Gets the fields specified.  // 获取指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if any field does not exist.  // 如果任何字段不存在，则抛出 InputError。

**参数**
field_ids – List of ids for the fields.  // 字段 ID 的列表。

**返回**
The fields.  // 返回字段。

```cpp
std::vector<FieldData> GetFields(const std::vector<size_t> &field_ids) const
```
Gets the fields specified.  // 获取指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if any field does not exist.  // 如果任何字段不存在，则抛出 InputError。

**参数**
field_ids – List of ids for the fields.  // 字段 ID 的列表。

**返回**
The fields.  // 返回字段。

```cpp
FieldData GetField(size_t field_id) const
```
Gets the field specified.  // 获取指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if field does not exist.  // 如果字段不存在，则抛出 InputError。

**参数**
field_id – Field ID.  // 字段 ID。

**返回**
Field value.  // 返回字段值。

```cpp
inline FieldData operator[](const std::string &field_name) const
```
Get field identified by field_name.  // 获取由字段名标识的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if field does not exist.  // 如果字段不存在，则抛出 InputError。

**参数**
field_name – Filename of the file.  // 文件名。

**返回**
The indexed value.  // 返回索引值。

```cpp
inline FieldData operator[](size_t fid) const
```
Get field identified by field id.  // 获取由字段 ID 标识的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if field does not exist.  // 如果字段不存在，则抛出 InputError。

**参数**
fid – The field id.  // 字段 ID。

**返回**
The indexed value.  // 返回索引值。

```cpp
std::map<std::string, FieldData> GetAllFields() const
```
Gets all fields of current vertex.  // 获取当前顶点的所有字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

InputError – Thrown if any field does not exist.  // 如果任何字段不存在，则抛出 InputError。

**返回**
All field names and values stored as a {(field_name, field_value),…} map.  // 返回以 {(字段名, 字段值), …} 存储的所有字段名和值的映射。

```cpp
void SetField(const std::string &field_name, const FieldData &field_value)
```
Sets the specified field.  // 设置指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

InputError – Thrown if any field does not exist.  // 如果任何字段不存在，则抛出 InputError。

**参数**
field_name – Field name.  // 字段名。

field_value – Field value.  // 字段值。

```cpp
void SetField(size_t field_id, const FieldData &field_value)
```
Sets the specified field.  // 设置指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

InputError – Thrown if field does not exist.  // 如果字段不存在，则抛出 InputError。

**参数**
field_id – Field id.  // 字段 ID。

field_value – Field value.  // 字段值。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Sets the fields specified.  // 设置指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

InputError – Thrown if any field does not exist or field value type is incorrect.  // 如果任何字段不存在或字段值类型不正确，则抛出 InputError。

**参数**
field_names – List of names of the fields.  // 字段名称的列表。

field_value_strings – The field value strings.  // 字段值字符串。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Sets the fields specified.  // 设置指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

InputError – Thrown if any field does not exist or field value type is incorrect.  // 如果任何字段不存在或字段值类型不正确，则抛出 InputError。

**参数**
field_names – List of names of the fields.  // 字段名称的列表。

field_values – The field values.  // 字段值。

```cpp
void SetFields(const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Sets the fields specified.  // 设置指定的字段。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

InputError – Thrown if any field does not exist or field value type is incorrect.  // 如果任何字段不存在或字段值类型不正确，则抛出 InputError。

**参数**
field_ids – List of identifiers for the fields.  // 字段的标识符列表。

field_values – The field values.  // 字段值。

```cpp
void Delete()
```
Deletes this edge. The iterator will point to the next incoming edge sorted by (lid, tid, src, eid) if there is any. If no in-coming edge is left for this vertex, the iterator becomes invalid.  // 删除这条边。如果有任何边，这个迭代器将指向下一个按 (lid, tid, src, eid) 排序的入边。如果此顶点没有剩余的入边，迭代器将变为无效。

**抛出**
InvalidTxn – Thrown if the transaction is invalid.  // 如果事务无效，则抛出 InvalidTxn。

InvalidIterator – Thrown if the iterator is invalid.  // 如果迭代器无效，则抛出 InvalidIterator。

WriteNotAllowed – Thrown if called inside a read-only transaction.  // 如果在只读事务中调用，则抛出 WriteNotAllowed。

```cpp
std::string ToString() const
```
Get string representation of the edge.  // 获取边的字符串表示。

### Private Functions

```cpp
InEdgeIterator(lgraph::graph::InEdgeIterator&&, const std::shared_ptr<lgraph::Transaction>&)
```
Constructors are private; use `Transaction::GetInEdgeIterator()` or `VertexIterator::GetInEdgeIterator()` instead.  
构造函数是私有的；请使用 `Transaction::GetInEdgeIterator()` 或 `VertexIterator::GetInEdgeIterator()` 代替。

```cpp
InEdgeIterator(const InEdgeIterator&) = delete
```
Deleted copy constructor.  
删除拷贝构造函数。

```cpp
InEdgeIterator &operator=(const InEdgeIterator&) = delete
```
Deleted copy assignment operator.  
删除拷贝赋值运算符。

#### Private Members

```cpp
std::unique_ptr<lgraph::graph::InEdgeIterator> it_
```
Unique pointer to an InEdgeIterator.  
指向 InEdgeIterator 的唯一指针。

```cpp
std::shared_ptr<lgraph::Transaction> txn_
```
Shared pointer to a Transaction.  
指向 Transaction 的共享指针。

#### Friends

```cpp
friend class Transaction
```
Friend class declaration for Transaction.  
Transaction 的友元类声明。

```cpp
friend class VertexIterator
```
Friend class declaration for VertexIterator.  
VertexIterator 的友元类声明。

### Class OutEdgeIterator

```cpp
#include <lgraph_edge_iterator.h>
```

An `OutEdgeIterator` can be used to iterate through the outgoing edges of a vertex. Edges are sorted in the order of (lid, dst, eid), and each (src, lid, tid, dst, eid) tuple uniquely identifies an edge.  
`OutEdgeIterator` 可以用于遍历一个顶点的出边。边按 (lid, dst, eid) 的顺序排序，每个 (src, lid, tid, dst, eid) 元组唯一标识一条边。

An `OutEdgeIterator` is valid if it points to a valid outgoing edge; otherwise, it is invalid. Calling member functions on an invalid `OutEdgeIterator` throws an `InvalidIterator`, except for the `IsValid()` and `Goto()` functions.  
如果 `OutEdgeIterator` 指向有效的出边，则它是有效的；否则，它是无效的。在无效的 `OutEdgeIterator` 上调用成员函数会抛出 `InvalidIterator`， `IsValid()` 和 `Goto()` 函数除外。

The following operations invalidate an `OutEdgeIterator`:
- Constructing an `OutEdgeIterator` for a non-existing edge.  
- 为不存在的边构造 `OutEdgeIterator`。
- Calling `Goto()` with the id of a non-existing edge.  
- 使用不存在边的 ID 调用 `Goto()`。
- Calling `Next()` on the last outgoing edge.  
- 在最后一条出边上调用 `Next()`。
- Calling `Delete()` on the last outgoing edge.  
- 在最后一条出边上调用 `Delete()`。
- Calling `Close()` on the iterator.  
- 在迭代器上调用 `Close()`。

In TuGraph, every iterator belongs to a transaction and can only be used when the transaction is valid. Calling member functions on an iterator inside an invalid transaction yields `InvalidTxn`, except for `Invalid()`.  
在 TuGraph 中，每个迭代器属于一个事务，并且只能在事务有效时使用。在无效事务中的迭代器上调用成员函数会导致 `InvalidTxn`， `Invalid()` 除外。

#### Public Functions

```cpp
OutEdgeIterator(OutEdgeIterator &&rhs)
```
Move constructor.  
移动构造函数。

```cpp
OutEdgeIterator &operator=(OutEdgeIterator &&rhs)
```
Move assignment operator.  
移动赋值运算符。

```cpp
~OutEdgeIterator()
```
Destructor.  
析构函数。

```cpp
void Close() noexcept
```
Closes this iterator. The iterator turns invalid after being closed.  
关闭此迭代器。迭代器在关闭后变为无效。

```cpp
bool Goto(EdgeUid euid, bool nearest = false)
```
Go to the edge specified by `euid`. If `nearest == true` and the exact edge was not found, the iterator tries to get the next edge that sorts after the specified edge. The edges are sorted in (label, tid, dst, eid) order. The iterator becomes invalid if there is no outgoing edge from `euid.src` that sorts after `euid`.  
转到由 `euid` 指定的边。如果 `nearest == true` 并且未找到确切的边，则迭代器尝试获取在指定边后排序的下一条边。边按 (label, tid, dst, eid) 顺序排序。如果从 `euid.src` 没有排在 `euid` 后面的出边，则迭代器变为无效。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。

**参数**  
- `euid` – Edge Unique ID.  
  边的唯一 ID。
- `nearest` – (Optional) True to get the nearest edge if the specified one cannot be found.  
  （可选）如果找不到指定的边，则为 true 以获取最近的边。

**返回**  
True if it succeeds, false if there is no such edge.  
成功则返回 true，如果没有这样的边则返回 false。

```cpp
bool IsValid() const noexcept
```
Query if this iterator is valid.  
查询此迭代器是否有效。

**返回**  
True if valid, false if not.  
有效则返回 true，无效则返回 false。

```cpp
bool Next()
```
Move to the next edge. Invalidates iterator if there are no more outgoing edges from the current source vertex.  
移动到下一条边。如果当前源顶点没有更多的出边，则使迭代器无效。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
True if it succeeds, false if it fails (no more out edges from current source).  
成功则返回 true，如果失败（当前源没有更多出边）则返回 false。

```cpp
EdgeUid GetUid() const
```
Gets the Edge Unique Id.  
获取边的唯一 ID。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The UID.  
唯一 ID。

```cpp
int64_t GetDst() const
```
Gets the destination of the edge.  
获取边的目的地。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The destination vertex id.  
目的地顶点 ID。

```cpp
int64_t GetEdgeId() const
```
Gets the edge id.  
获取边的 ID。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The edge identifier.  
边的标识符。

```cpp
int64_t GetTemporalId() const
```
Gets the primary id.  
获取主 ID。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The primary id of the edge.  
边的主 ID。

```cpp
int64_t GetSrc() const
```
Gets the source vertex id.  
获取源顶点 ID。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The source vertex id.  
源顶点 ID。

```cpp
const std::string &GetLabel() const
```
Gets the label of this edge.  
获取该边的标签。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The label.  
标签。

```cpp
int16_t GetLabelId() const
```
Gets the label id of this edge.  
获取该边的标签 ID。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。

**返回**  
The label identifier.  
标签标识符。

```cpp
std::vector<FieldData> GetFields(const std::vector<std::string> &field_names) const
```
Gets the fields specified.  
获取指定的字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

**参数**  
- `field_names` – List of names of the fields.  
  字段名称列表。

**返回**  
The fields.  
字段。

```cpp
FieldData GetField(const std::string &field_name) const
```
Gets the field specified.  
获取指定的字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid.  
当当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

**参数**  
- `field_name` – Field name.  
  字段名称。

**返回**  
Field value.  
字段值。

```cpp
std::vector<FieldData> GetFields(const std::vector<size_t> &field_ids) const
```
Gets the fields specified.  // 获取指定的字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_ids` – List of ids for the fields. // 字段的ID列表。

**返回**  
The fields. // 字段。

```cpp
FieldData GetField(size_t field_id) const
```
Gets the field specified. // 获取指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_id` – Field ID. // 字段ID。

**返回**  
Field value. // 字段值。

```cpp
inline FieldData operator[](const std::string &field_name) const
```
Get field identified by field_name. // 获取由 field_name 标识的字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_name` – The name of the field to get. // 要获取的字段名称。

**返回**  
Field value. // 字段值。

```cpp
inline FieldData operator[](size_t fid) const
```
Get field identified by field id. FieldId can be obtained with `txn.GetEdgeFieldId()`. // 获取由字段ID标识的字段。FieldId 可以通过 `txn.GetEdgeFieldId()` 获取。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `fid` – Field id. // 字段ID。

**返回**  
Field value. // 字段值。

```cpp
std::map<std::string, FieldData> GetAllFields() const
```
Gets all fields of the current vertex. // 获取当前顶点的所有字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。

**返回**  
All fields in a dictionary of {(field_name, field_value),…}. // 返回一个字典，其中包含所有字段 {(字段名称, 字段值),…}。

```cpp
void SetField(const std::string &field_name, const FieldData &field_value)
```
Sets the specified field. // 设置指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_name` – Field name. // 字段名称。
- `field_value` – Field value. // 字段值。

```cpp
void SetField(size_t field_id, const FieldData &field_value)
```
Sets the specified field. // 设置指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_id` – Field id. // 字段ID。
- `field_value` – Field value. // 字段值。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Sets the fields specified. // 设置指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_names` – List of names of the fields. // 字段名称列表。
- `field_value_strings` – The field values in string representation. // 字段值的字符串表示。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Sets the fields specified. // 设置指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_names` – List of names of the fields. // 字段名称列表。
- `field_values` – The field values. // 字段值。

```cpp
void SetFields(const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Sets the fields specified. // 设置指定字段。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。  
InputError – Thrown on other input errors (field not exist, etc.). // 在其他输入错误时抛出（字段不存在等）。

**参数**  
- `field_ids` – List of identifiers for the fields. // 字段的标识符列表。
- `field_values` – The field values. // 字段值。

```cpp
void Delete()
```
Deletes this edge. The iterator will point to the next outgoing edge sorted by (label, tid, dst, eid) if there is any. If there are no more outgoing edges for this source vertex, the iterator becomes invalid. // 删除该边。如果有，迭代器将指向下一个按 (label, tid, dst, eid) 排序的出边。如果该源顶点没有更多出边，迭代器将变得无效。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。  
WriteNotAllowed – Thrown when called in a read-only transaction. // 在只读事务中调用时抛出。

```cpp
std::string ToString() const
```
Get string representation of the edge. // 获取边的字符串表示。

**抛出**  
InvalidTxn – Thrown when called inside an invalid transaction. // 当在无效事务中调用时抛出。  
InvalidIterator – Thrown when the current iterator is invalid. // 当前迭代器无效时抛出。

**返回**  
A `std::string` that represents this object. // 一个表示该对象的 `std::string`。

### Private Functions

```cpp
OutEdgeIterator(lgraph::graph::OutEdgeIterator&&, const std::shared_ptr<lgraph::Transaction>&)
```
Constructors are private; use `Transaction::GetOutEdgeIterator()` or `VertexIterator::GetOutEdgeIterator()` instead. // 构造函数是私有的；请使用 `Transaction::GetOutEdgeIterator()` 或 `VertexIterator::GetOutEdgeIterator()`。

```cpp
OutEdgeIterator(const OutEdgeIterator&) = delete
```
This copy constructor is deleted to prevent copying. // 该复制构造函数被删除以防止复制。

```cpp
OutEdgeIterator &operator=(const OutEdgeIterator&) = delete
```
This assignment operator is deleted to prevent assignment. // 该赋值运算符被删除以防止赋值。

#### Private Members

```cpp
std::unique_ptr<lgraph::graph::OutEdgeIterator> it_
```
Unique pointer to the out-edge iterator. // 指向出边迭代器的唯一指针。

```cpp
std::shared_ptr<lgraph::Transaction> txn_
```
Shared pointer to the transaction. // 指向事务的共享指针。

#### Friends

```cpp
friend class Transaction
```
Transaction class is a friend. // Transaction 类是友元类。

```cpp
friend class VertexIterator
```
VertexIterator class is a friend. // VertexIterator 类是友元类。














## lgraph_exceptions

### Defines

- **ERROR_CODES**  // 定义了错误代码
- **X(code, msg)**  // 定义了一个宏，接受错误代码和消息
- **THROW_CODE(code, ...)**  // 定义了一个宏，用于抛出指定的错误代码

### namespace lgraph_api  // 命名空间 lgraph_api

### Enums

- **enum class ErrorCode**  // 错误代码的枚举类
  - Values:  // 枚举值
    - enumerator X  // 枚举值 X
    - enumerator ERROR_CODES  // 枚举值 ERROR_CODES

### Functions

```cpp
const char *ErrorCodeToString(ErrorCode code)  // 将错误代码转换为字符串
```

```cpp
const char *ErrorCodeDesc(ErrorCode code)  // 获取错误代码的描述
```

### class LgraphException : public exception  // LgraphException 类，继承自标准异常类

```cpp
#include <lgraph_exceptions.h>  // 引入 lgraph_exceptions 头文件
```

#### Public Functions

```cpp
explicit LgraphException(ErrorCode code)  // 构造函数，接受一个错误代码
```

```cpp
explicit LgraphException(ErrorCode code, const std::string &msg)  // 构造函数，接受错误代码和消息字符串
```

```cpp
explicit LgraphException(ErrorCode code, const char *msg)  // 构造函数，接受错误代码和消息 C 字符串
```

```cpp
template<typename ...Ts>
inline explicit LgraphException(ErrorCode code, const char *format, const Ts&... ds)  // 模板构造函数，支持格式化消息
```

```cpp
inline ErrorCode code() const  // 返回错误代码
```

```cpp
inline const std::string &msg() const  // 返回错误消息字符串
```

```cpp
inline const char *what() const noexcept override  // 重写 what() 函数，返回异常的描述
```

#### Private Members

- **ErrorCode code_**  // 存储错误代码的私有成员
- **std::string msg_**  // 存储错误消息的私有成员
- **std::string what_**  // 存储异常描述的私有成员




















## lgraph_galaxy

### namespace lgraph
### namespace lgraph_api

### class Galaxy

```cpp
#include <lgraph_galaxy.h>
```

A galaxy is the storage engine for one TuGraph instance. It manages a set of User/Role/GraphDBs.
  
一个 galaxy 是一个 TuGraph 实例的存储引擎。它管理一组用户/角色/图数据库。

A galaxy can be opened in async mode, in which case ALL write transactions will be treated as async, whether they declare async or not. This can come in handy if we are performing a lot of writing, but can cause data loss for online processing.

一个 galaxy 可以在异步模式下打开，在这种情况下，所有写事务都将被视为异步，无论它们是否声明为异步。这在进行大量写入时可能会很有用，但可能会导致在线处理中的数据丢失。

#### Public Functions

```cpp
explicit Galaxy(const std::string &dir, bool durable = false, bool create_if_not_exist = true)
```
Constructor.

构造函数。

抛出  
DBNotExist – Thrown if DB does not exist and create_if_not_exist is false.

此异常在数据库不存在且 create_if_not_exist 为 false 时抛出。

IOError – Thrown if DB does not exist, but we failed to create the DB due to IO error.

此异常在数据库不存在，但由于 IO 错误无法创建数据库时抛出。

InputError – Thrown if there are other input errors. e.g., dir is actually a plain file, or DB is corruptted.

此异常在存在其他输入错误时抛出。例如，dir 实际上是一个普通文件，或数据库已损坏。

参数  
dir – The TuGraph dir.

dir – TuGraph 的目录。

durable – (Optional) True to open in durable mode. If set to false, ALL write transactions are async, whether they declare async or not.

durable – （可选）设置为 true 以在持久模式下打开。如果设置为 false，所有写事务都是异步的，无论它们是否声明为异步。

create_if_not_exist – (Optional) If true, the TuGraph DB will be created if dir does not exist; otherwise, an exception is thrown.

create_if_not_exist – （可选）如果为 true，且 dir 不存在，则将创建 TuGraph 数据库；否则将抛出异常。

```cpp
Galaxy(const std::string &dir, const std::string &user, const std::string &password, bool durable, bool create_if_not_exist)
```
Constructor. Open the Galaxy and try to login with specified user and password.

构造函数。打开 Galaxy 并尝试使用指定的用户和密码登录。

抛出  
DBNotExist – Thrown if DB does not exist and create_if_not_exist is false.

此异常在数据库不存在且 create_if_not_exist 为 false 时抛出。

IOError – Thrown if DB does not exist, but we failed to create the DB due to IO error.

此异常在数据库不存在，但由于 IO 错误无法创建数据库时抛出。

InputError – Thrown if there are other input errors. e.g., dir is actually a plain file, or DB is corruptted.

此异常在存在其他输入错误时抛出。例如，dir 实际上是一个普通文件，或数据库已损坏。

Unauthorized – Thrown if user/password is not correct.

当用户/密码不正确时抛出此异常。

参数  
dir – The dir.

dir – 目录。

user – The user.

user – 用户。

password – The password.

password – 密码。

durable – True to open the Galaxy in durable mode.

durable – 设置为 true 以在持久模式下打开 Galaxy。

create_if_not_exist – True to create if DB does not exist.

create_if_not_exist – 如果数据库不存在，则为 true，以创建数据库。

```cpp
Galaxy(Galaxy&&)
```

```cpp
Galaxy &operator=(Galaxy&&)
```

```cpp
~Galaxy()
```

```cpp
void SetCurrentUser(const std::string &user, const std::string &password)
```
Validate and set current user

验证并设置当前用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user/password is incorrect.

当用户/密码不正确时抛出此异常。

参数  
user – The user.

user – 用户。

password – The password.

password – 密码。

```cpp
void SetUser(const std::string &user)
```
Set current user

设置当前用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if token is incorrect.

当令牌不正确时抛出此异常。

参数  
user – The current user.

user – 当前用户。

```cpp
bool CreateGraph(const std::string &graph_name, const std::string &description = "", size_t max_size = (size_t)1 << 40)
```
Validate token and set current user

验证令牌并设置当前用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission to create graph.

当用户没有创建图表的权限时抛出此异常。

InputError – Other input errors such as invalid graph name, size, etc.

输入错误 – 其他输入错误，例如无效的图表名称、大小等。

参数  
graph_name – Name of the graph to create.  
description (Optional) – Description of the graph.  
max_size (Optional) – Maximum size of the graph.

返回  
True if it succeeds, false if graph already exists.

如果成功返回 true，如果图形已经存在则返回 false。

```cpp
bool DeleteGraph(const std::string &graph_name)
```
Delete a graph

删除一张图。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission to delete graph.

当用户没有删除图表的权限时抛出此异常。

参数  
graph_name – Name of the graph.

graph_name – 图形名称。

返回  
True if it succeeds, false if the graph does not exist.

如果成功返回 true，如果图形不存在则返回 false。

```cpp
bool ModGraph(const std::string &graph_name, bool mod_desc, const std::string &desc, bool mod_size, size_t new_max_size)
```
Modify graph info

修改图信息。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission to modify graph.

当用户没有修改图表的权限时抛出此异常。

参数  
graph_name – Name of the graph.  
mod_desc – True to modify description.  
desc – The new description.  
mod_size – True to modify size.  
new_max_size – New maximum size.

返回  
True if it succeeds, false if it fails.

如果成功返回 true，如果失败返回 false。

```cpp
std::map<std::string, std::pair<std::string, size_t>> ListGraphs() const
```
List graphs

列出图表。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission to list graphs.

当用户没有列出图表的权限时抛出此异常。

返回  
A dictionary of {graph_name: (description, max_size)}

返回一个字典，格式为 {graph_name: (description, max_size)}。

```cpp
bool CreateUser(const std::string &user, const std::string &password, const std::string &desc = "")
```
Creates a user

创建用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

InputError – Thrown if other input errors, such as illegal user name, password, etc.

输入错误 – 如果存在其他输入错误，例如非法的用户名、密码等，抛出此异常。

参数  
user – The user.  
password – The password.  
desc – (Optional) The description.

返回  
True if it succeeds, false if user already exists.

如果成功返回 true，如果用户已经存在则返回 false。

```cpp
bool DeleteUser(const std::string &user)
```
Deletes the user.

删除用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

参数  
user – The user.

user – 用户。

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool SetPassword(const std::string &user, const std::string &old_password, const std::string &new_password)
```
Set the password of the specified user.

设置指定用户的密码。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission, or curr_user==user, but old_password is incorrect.

当用户没有权限时抛出此异常，或当前用户 == 用户，但是旧密码不正确。

InputError – Thrown if new_password is illegal.

输入错误 – 当 new_password 不合法时抛出此异常。

参数  
user – The user to modify.  
old_password – The old password, required if curr_user==user.  
new_password – The new password.

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool SetUserDesc(const std::string &user, const std::string &desc)
```
Sets user description.

设置用户描述。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

InputError – Thrown if desc is illegal.

输入错误 – 当 desc 不合法时抛出此异常。

参数  
user – The user.  
desc – The new description.

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool SetUserRoles(const std::string &user, const std::vector<std::string> &roles)
```
Set the roles of the specified user. If you need to add or delete a role, you will need to use GetUserInfo to get the roles first.

设置指定用户的角色。如果需要添加或删除角色，则首先需要使用 GetUserInfo 获取角色。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

InputError – Thrown if any role does not exist.

输入错误 – 如果任何角色不存在则抛出此异常。

参数  
user – The user.  
roles – A list of roles.

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool SetUserGraphAccess(const std::string &user, const std::string &graph, const AccessLevel &access)
```
Sets user access rights on a graph.

设置用户对图表的访问权限。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

InputError – Thrown if graph does not exist.

输入错误 – 如果图形不存在则抛出此异常。

参数  
user – The user.  
graph – The graph.  
access – The access level.

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool DisableUser(const std::string &user)
```
Disable a user. A disabled user is not able to login or perform any operation. A user cannot disable itself.

禁用用户。被禁用的用户无法登录或执行任何操作。用户无法禁用自己。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid.

当当前 galaxy 无效时抛出此异常。

Unauthorized – Thrown if user does not have permission.

当用户没有权限时抛出此异常。

InputError – Thrown if user name is illegal.

输入错误 – 当用户名非法时抛出此异常。

参数  
user – The user to disable.

user – 要禁用的用户。

返回  
True if it succeeds, false if user does not exist.

如果成功返回 true，如果用户不存在则返回 false。

```cpp
bool EnableUser(const std::string &user)
```
Enables the user. // 启用用户。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if user name is illegal. // 如果用户名不合法，则抛出 InputError。

参数  
user – The user. // user – 用户名。

返回  
True if it succeeds, false if user does not exist. // 成功返回 True，如果用户不存在则返回 false。

```cpp
std::map<std::string, UserInfo> ListUsers() const
```
List all users // 列出所有用户

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

返回  
A dictionary of {user_name:user_info} // 返回一个字典，格式为 {用户名:用户信息}

```cpp
UserInfo GetUserInfo(const std::string &user) const
```
Gets user information // 获取用户信息

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

参数  
user – The user. // user – 用户名。

返回  
The user information. // 返回用户信息。

```cpp
bool CreateRole(const std::string &role, const std::string &desc)
```
Create a role. A role has different access levels to different graphs. Every user must be assigned some role to get access to graphs. // 创建一个角色。角色对不同图形具有不同的访问级别。每个用户必须被分配某个角色才能访问图形。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name or desc is illegal. // 如果角色名称或描述不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。  
desc – The description. // desc – 描述。

返回  
True if it succeeds, false if role already exists. // 成功返回 True，如果角色已存在则返回 false。

```cpp
bool DeleteRole(const std::string &role)
```
Deletes the role described by role // 删除由角色描述的角色

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name is illegal. // 如果角色名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。

返回  
True if it succeeds, false if role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
bool DisableRole(const std::string &role)
```
Disable a role. A disabled role still has the data, but is not effective. i.e., users will not have access rights to graphs that are obtained by having this role. // 禁用一个角色。禁用的角色仍然有数据，但无效。即，用户将无法访问通过拥有该角色获得的图形。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name is illegal. // 如果角色名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。

返回  
True if it succeeds, false if the role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
bool EnableRole(const std::string &role)
```
Enables the role. // 启用角色。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name is illegal. // 如果角色名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。

返回  
True if it succeeds, false if role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
bool SetRoleDesc(const std::string &role, const std::string &desc)
```
Set the description of the specified role // 设置指定角色的描述

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name or desc is illegal. // 如果角色名称或描述不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。  
desc – The description. // desc – 描述。

返回  
True if it succeeds, false if role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
bool SetRoleAccessRights(const std::string &role, const std::map<std::string, AccessLevel> &graph_access)
```
Set access of the role to graphs. If you need to add or remove access to part of the graphs, you need to get full graph_access map by using GetRoleInfo first. // 设置角色对图形的访问权限。如果需要添加或删除对部分图形的访问权限，必须先通过 GetRoleInfo 获取完整的 graph_access 映射。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name or any of the graph name is illegal. // 如果角色名称或任何图形名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。  
graph_access – The graph access. // graph_access – 图形访问权限。

返回  
True if it succeeds, false if role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
bool SetRoleAccessRightsIncremental(const std::string &role, const std::map<std::string, AccessLevel> &graph_access)
```
Incrementally modify the access right of the specified role. For example, for a role that has access right {graph1:READ, graph2:WRITE}, calling this function with graph_access={graph2:READ, graph3:FULL} will set the access right of this role to {graph1:READ, graph2:READ, graph3:FULL} // 逐步修改指定角色的访问权限。例如，对于一个访问权限为 {graph1:READ, graph2:WRITE} 的角色，调用此函数并传入 graph_access={graph2:READ, graph3:FULL} 将把该角色的访问权限设置为 {graph1:READ, graph2:READ, graph3:FULL}。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name or any of the graph name is illegal. // 如果角色名称或任何图形名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。  
graph_access – The graph access. // graph_access – 图形访问权限。

返回  
True if it succeeds, false if role does not exist. // 成功返回 True，如果角色不存在则返回 false。

```cpp
RoleInfo GetRoleInfo(const std::string &role) const
```
Gets role information // 获取角色信息

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if role name is illegal. // 如果角色名称不合法，则抛出 InputError。

参数  
role – The role. // role – 角色名称。

返回  
The role information. // 返回角色信息。

```cpp
std::map<std::string, RoleInfo> ListRoles() const
```
List all the roles // 列出所有角色

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

返回  
A dictionary of {role_name:RoleInfo} // 返回一个字典，格式为 {角色名称:角色信息}

```cpp
AccessLevel GetAccessLevel(const std::string &user, const std::string &graph) const
```
Get the access level that the specified user have to the graph // 获取指定用户对图形的访问级别

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if user name or graph name is illegal. // 如果用户名或图形名称不合法，则抛出 InputError。

参数  
user – The user. // user – 用户名。  
graph – The graph. // graph – 图形名称。

返回  
The access level. // 返回访问级别。

```cpp
GraphDB OpenGraph(const std::string &graph, bool read_only = false) const
```
Opens a graph. // 打开一个图形。

抛出  
InvalidGalaxy – Thrown if current galaxy is invalid. // 如果当前的 galaxy 无效，则抛出 InvalidGalaxy。

Unauthorized – Thrown if user does not have permission. // 如果用户没有权限，则抛出 Unauthorized。

InputError – Thrown if graph name is illegal. // 如果图形名称不合法，则抛出 InputError。

参数  
graph – The graph. // graph – 图形名称。  
read_only – (Optional) True to open in read-only mode. A read-only GraphDB cannot be written to. // read_only – （可选）若为 True，则以只读模式打开。只读的 GraphDB 不能被写入。

返回  
A GraphDB. // 返回一个 GraphDB。

```cpp
void Close()
```
Closes this Galaxy, turning it into an invalid state. // 关闭这个 Galaxy，将其变为无效状态。

#### Private Functions

```cpp
explicit Galaxy(lgraph::Galaxy *db)
```

```cpp
inline Galaxy(const Galaxy&)
```

```cpp
inline Galaxy &operator=(const Galaxy&)
```

#### Private Members

```cpp
std::string user_
```

```cpp
lgraph::Galaxy *db_
```












## lgraph_result

Result interface for plugins and built-in procedures. The result of a plugin should be provided in this format in order for the Cypher engine and the graph visualizer to understand.  
结果接口用于插件和内置过程。插件的结果应以这种格式提供，以便 Cypher 引擎和图形可视化工具能够理解。

### Namespace
- `namespace lgraph`  
- `namespace cypher`  
- `namespace lgraph_api`  

### Typedefs
```cpp
typedef std::unordered_map<size_t, std::shared_ptr<lgraph_result::Node>> NODEMAP
```
```cpp
typedef std::unordered_map<EdgeUid, std::shared_ptr<lgraph_result::Relationship>, EdgeUid::Hash> RELPMAP
```

### Class Record
```cpp
#include <lgraph_result.h>
```
You only initialize the class by Result instance. Record provides some insert methods to insert data into the record, e.g., Insert, InsertVertexByID, InsertEdgeByID.  
您只能通过 Result 实例初始化类。Record 提供了一些插入方法将数据插入记录，例如：Insert、InsertVertexByID、InsertEdgeByID。

#### Public Functions
```cpp
Record(const Record&)
```
```cpp
Record(Record&&)
```
```cpp
Record &operator=(const Record&)
```
```cpp
Record &operator=(Record&&)
```
```cpp
void Insert(const std::string &fname, const FieldData &fv)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – Field name you defined earlier.  // fname - 您之前定义的字段名  
fv – Field value.  // fv - 字段值

```cpp
void Insert(const std::string &fname, const int64_t vid, lgraph_api::Transaction *txn)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier. You can get properties and labels from the interface; this is different from InsertVertexByID.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。您可以通过接口获取属性和标签，这与 InsertVertexByID 不同。

参数  
fname – Title name you defined earlier.  // fname - 您之前定义的标题名称  
vid – VertexId  // vid - 顶点 ID  
txn – Transaction  // txn - 事务  

```cpp
void Insert(const std::string &fname, EdgeUid &euid, lgraph_api::Transaction *txn)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier. You can get properties and labels from the interface; this is different from InsertEdgeByID.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。您可以通过接口获取属性和标签，这与 InsertEdgeByID 不同。

参数  
fname – Title name you defined earlier.  // fname - 您之前定义的标题名称  
euid – EdgeUid  // euid - 边 UID  
txn – Transaction  // txn - 事务  

```cpp
void InsertVertexByID(const std::string &fname, int64_t vid)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – Field name you defined earlier.  // fname - 您之前定义的字段名  
vid – VertexId.  // vid - 顶点 ID

```cpp
void InsertEdgeByID(const std::string &fname, const EdgeUid &uid)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – Title name you defined earlier.  // fname - 您之前定义的标题名称  
uid – EdgeUid.  // uid - 边 UID

```cpp
void Insert(const std::string &fname, const lgraph_api::VertexIterator &vit)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
vit – VertexIterator.  // vit - 顶点迭代器

```cpp
void Insert(const std::string &fname, const lgraph_api::InEdgeIterator &ieit)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
ieit – InEdgeIterator.  // ieit - 入边迭代器

```cpp
void Insert(const std::string &fname, const lgraph_api::OutEdgeIterator &oeit)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
oeit – OutEdgeIterator.  // oeit - 出边迭代器

```cpp
void Insert(const std::string &fname, const std::vector<FieldData> &list)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
list – List of FieldData.  // list - FieldData 的列表

```cpp
void Insert(const std::string &fname, const std::map<std::string, FieldData> &map)
```
Insert a value to the result table. You can insert a value by the insert function, and the value type must be the same as you defined earlier.  
将值插入结果表。您可以通过插入函数插入一个值，且值的类型必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
map – Map of <string, FieldData>  // map - <string, FieldData> 的映射

```cpp
void Insert(const std::string &fname, const traversal::Path &path, lgraph_api::Transaction *txn, NODEMAP &node_map, RELPMAP &relp_map)
```
Insert value into the result table. You can insert a value by the function, and the value must be the same as you defined earlier.  
将值插入结果表。您可以通过该函数插入值，且该值必须与您之前定义的相同。

参数  
fname – One of the title names you defined earlier.  // fname - 您之前定义的标题名称之一  
path – Path of traverse API.  // path - 遍历 API 的路径  
txn – Transaction  // txn - 事务  

```cpp
inline int64_t Size() const
```
Get the size of the record. If the record is empty, return 0; max size is not beyond the length of your defined param list.  
获取记录的大小。如果记录为空，则返回 0；最大大小不超过您定义的参数列表的长度。

返回  
Size of record.  // 返回：记录的大小

```cpp
inline bool HasKey(const std::string &key) const
```
Check if a key exists or not. The key is one of the titles you defined earlier.  
检查某个键是否存在。该键是您之前定义的标题之一。

参数  
key – The key.  // key - 键

返回  
True if exists, otherwise false.  // 如果存在则返回真，否则返回假。

#### Private Functions
```cpp
explicit Record(const std::vector<std::pair<std::string, LGraphType>>&)
```

#### Private Members
```cpp
std::unordered_map<std::string, std::shared_ptr<ResultElement>> record
```
```cpp
std::unordered_map<std::string, LGraphType> header
```
```cpp
int64_t length_
```

#### Friends
friend class Result

### Class Result
```cpp
#include <lgraph_result.h>
```
Result table, result instance provides [MutableRecord], [ResetHeader], [Dump], and [Load] methods. The table also provides some methods to view the content of the table. For example, [Header] and [Recordview].  
结果表，结果实例提供 [MutableRecord]、[ResetHeader]、[Dump] 和 [Load] 方法。该表还提供一些方法以查看表的内容。例如， [Header] 和 [Recordview]。

It’s worth noting that you are best to define your header before using the result table. e.g., `auto result = Result({title, LGraphType}…)` If you do not define the header and initialize by Result(), you will get an empty table without a header; you just use the table after using [ResetHeader] method to set your header.  
值得注意的是，您最好在使用结果表之前定义您的头部。例如， `auto result = Result({title, LGraphType}…)` 如果您没有定义头部并通过 Result() 初始化，您将得到一个没有头部的空表；您只能在使用 [ResetHeader] 方法设置头部后使用该表。

#### Public Functions
```cpp
Result()
```
构造函数，初始化一个结果对象。

```cpp
Result(const std::initializer_list<std::pair<std::string, LGraphType>>&)
```
使用初始化列表构造结果对象。

```cpp
explicit Result(const std::vector<std::pair<std::string, LGraphType>>&)
```
使用向量构造结果对象，参数为一对字符串和LGraphType的向量。

```cpp
LGraphType GetType(std::string title)
```
Get the type of the title.  
获取标题的类型。

参数  
title – One of the titles in the table.  
标题 - 表格中的一个标题。

返回  
LGraphType.  
返回LGraphType。

```cpp
void ResetHeader(const std::vector<std::pair<std::string, LGraphType>> &header)
```
Reset your header; the operation will clear the original data and header. Please use the function carefully.  
重置你的表头；该操作将清除原始数据和表头。请小心使用该函数。

参数  
header – List of <title, LGraphType>  
表头 – <标题, LGraphType> 的列表。

```cpp
void ResetHeader(const std::initializer_list<std::pair<std::string, LGraphType>> &header)
```
Reset your header; the operation will clear the original data and header. Please use the function carefully.  
重置你的表头；该操作将清除原始数据和表头。请小心使用该函数。

参数  
header – List of <title, LGraphType>  
表头 – <标题, LGraphType> 的列表。

```cpp
Record *MutableRecord()
```
Create a new record in the table and return the record. The record is a reference of the record in the table; if you want to modify the record, you must assign the return value to a reference variable.  
在表中创建新记录并返回该记录。该记录是表中记录的引用；如果要修改该记录，必须将返回值分配给引用变量。

返回  
The reference of the record.  
记录的引用。

```cpp
void Reserve(size_t n)
```
This function attempts to reserve enough memory for the result vector to hold the specified number of elements.  
该函数尝试为结果向量预留足够的内存以容纳指定数量的元素。

```cpp
void Resize(size_t n)
```
This function will resize the vector to the specified number of elements.  
该函数将向量调整为指定数量的元素。

```cpp
Record *At(size_t n)
```
Provides access to the data contained in the vector.  
提供对向量中包含的数据的访问。

```cpp
const std::vector<std::pair<std::string, LGraphType>> &Header() const
```
Return the header of the table.  
返回表头。

返回  
Header.  
表头。

```cpp
int64_t Size() const
```
Return size of the table.  
返回表的大小。

返回  
Table size.  
表的大小。

```cpp
std::string Dump(bool is_standard = true)
```
Serialize the table.  
序列化表。

参数  
is_standard – (Optional) If true, the result will serialize to a standard result; the standard result can be visualized in the web. If false, the result will serialize a JSON object — SDK result.  
is_standard – （可选）如果为true，结果将序列化为标准结果；标准结果可以在网页上可视化。如果为false，结果将序列化为JSON对象 - SDK结果。

返回  
Serialize result.  
序列化结果。

```cpp
void Load(const std::string &json)
```
Deserialize data to the result table. This will clear the original data and header; please use this function carefully.  
将数据反序列化到结果表。 这将清除原始数据和表头；请小心使用此函数。

参数  
json – JSON string to be deserialized.  
json – 要反序列化的JSON字符串。

```cpp
void ClearRecords()
```
Clear all the records; Size() will be 0.  
清除所有记录；Size()将为0。

```cpp
std::vector<std::string> BoltHeader()
```
```cpp
std::vector<std::vector<std::any>> BoltRecords()
```
```cpp
inline void MarkPythonDriver(bool is_python_driver)
```
Mark that the result is returned to the Python driver. The Python driver is special, use the virtual edge id instead of the real edge id.  
标记结果已返回给Python驱动程序。Python驱动程序是特殊的，使用虚拟边缘ID而不是实际边缘ID。

#### Private Functions
```cpp
const std::unordered_map<std::string, std::shared_ptr<ResultElement>> &RecordView(int64_t row_num)
```
Get record of the table; if row number is more significant than the max length of the table, throw an exception.  
获取表的记录；如果行号大于表的最大长度，抛出异常。

参数  
row_num – Row number of the table.  
row_num – 表的行号。

返回  
One Record.  
一个记录。

#### Private Members
```cpp
std::vector<Record> result
```
记录结果的向量。

```cpp
std::vector<std::pair<std::string, LGraphType>> header
```
表头，存储标题与LGraphType的配对。

```cpp
int64_t row_count_
```
记录行数。

```cpp
bool is_python_driver_ = false
```
指示是否为Python驱动程序的标志，默认为false。

```cpp
int64_t v_eid_ = 0
```
虚拟边缘ID，默认为0。

#### Friends
friend class lgraph::StateMachine  
friend class cypher::PluginAdapter  
namespace lgraph_result  












## lgraph_rpc_client

```cpp
namespace fma_common
namespace lgraph_rpc
namespace lgraph
```

### Enums

- **enum class GraphQueryType**
  
  **Values:**
  - enumerator CYPHER  // 数据查询语言的枚举值，表示 Cypher 查询语言。
  - enumerator GQL     // 数据查询语言的枚举值，表示 GQL 查询语言。

- **enum ClientType**

  **Values:**
  - enumerator DIRECT_HA_CONNECTION      // 客户端类型，表示直接高可用性连接。
  - enumerator INDIRECT_HA_CONNECTION     // 客户端类型，表示间接高可用性连接。
  - enumerator SINGLE_CONNECTION           // 客户端类型，表示单一连接。

### class RpcClient

```cpp
#include <lgraph_rpc_client.h>
```

### Public Functions

- **explicit RpcClient(const std::string &url, const std::string &user, const std::string &password)**  
  RpcClient Login.  // 构造函数，用于登录。

  **参数**
  - url – Login address.  // 登录地址。
  - user – The username.  // 用户名。
  - password – The password.  // 密码。

- **explicit RpcClient(std::vector<std::string> &urls, std::string user, std::string password)**  
  RpcClient Login.  // 构造函数，用于登录。

  **参数**
  - urls – Login address.  // 登录地址。
  - user – The username.  // 用户名。
  - password – The password.  // 密码。

- **~RpcClient()**  // 析构函数。

- **bool CallCypher(std::string &result, const std::string &cypher, const std::string &graph = "default", bool json_format = true, double timeout = 0, const std::string &url = "")**  
  Execute a cypher query.  // 执行 Cypher 查询。

  **参数**
  - result – [out] The result.  // 输出结果。
  - cypher – [in] inquire statement.  // 输入查询语句。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。
  - url – [in] (Optional) Node address of calling cypher.  // 可选，调用 Cypher 的节点地址。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool CallCypherToLeader(std::string &result, const std::string &cypher, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Execute a cypher query to leader.  // 执行 Cypher 查询到领导者。

  **参数**
  - result – [out] The result.  // 输出结果。
  - cypher – [in] inquire statement.  // 输入查询语句。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool CallGql(std::string &result, const std::string &gql, const std::string &graph = "default", bool json_format = true, double timeout = 0, const std::string &url = "")**  
  Execute a gql query.  // 执行 GQL 查询。

  **参数**
  - result – [out] The result.  // 输出结果。
  - gql – [in] inquire statement.  // 输入查询语句。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。
  - url – [in] (Optional) Node address of calling cypher.  // 可选，调用 GQL 的节点地址。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool CallGqlToLeader(std::string &result, const std::string &gql, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Execute a gql query to leader.  // 执行 GQL 查询到领导者。

  **参数**
  - result – [out] The result.  // 输出结果。
  - gql – [in] inquire statement.  // 输入查询语句。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool CallProcedure(std::string &result, const std::string &procedure_type, const std::string &procedure_name, const std::string &param, double procedure_time_out = 0.0, bool in_process = false, const std::string &graph = "default", bool json_format = true, const std::string &url = "")**  
  Execute a user-defined procedure.  // 执行用户自定义的过程。

  **参数**
  - result – [out] The result.  // 输出结果。
  - procedure_type – [in] the procedure type, currently supported CPP and PY.  // 输入过程类型，目前支持 CPP 和 PY。
  - procedure_name – [in] procedure name.  // 输入过程名称。
  - param – [in] the execution parameters.  // 输入执行参数。
  - procedure_time_out – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。
  - in_process – [in] (Optional) support in future.  // 可选，未来支持。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。
  - url – [in] (Optional) Node address of calling procedure.  // 可选，调用过程的节点地址。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool CallProcedureToLeader(std::string &result, const std::string &procedure_type, const std::string &procedure_name, const std::string &param, double procedure_time_out = 0.0, bool in_process = false, const std::string &graph = "default", bool json_format = true)**  
  Execute a user-defined procedure to leader.  // 执行用户自定义的过程到领导者。

  **参数**
  - result – [out] The result.  // 输出结果。
  - procedure_type – [in] the procedure type, currently supported CPP and PY.  // 输入过程类型，目前支持 CPP 和 PY。
  - procedure_name – [in] procedure name.  // 输入过程名称。
  - param – [in] the execution parameters.  // 输入执行参数。
  - procedure_time_out – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 可选，最大执行时间，超时将中断。
  - in_process – [in] (Optional) support in future.  // 可选，未来支持。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 可选，返回格式，true 表示 JSON，否则为二进制格式。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool LoadProcedure(std::string &result, const std::string &source_file, const std::string &procedure_type, const std::string &procedure_name, const std::string &code_type, const std::string &procedure_description, bool read_only, const std::string &version = "v1", const std::string &graph = "default")**  
  Load a built-in procedure.  // 加载内置过程。

  **参数**
  - result – [out] The result.  // 输出结果。
  - source_file – [in] the source_file contain procedure code.  // 输入包含过程代码的源文件。
  - procedure_type – [in] the procedure type, currently supported CPP and PY.  // 输入过程类型，目前支持 CPP 和 PY。
  - procedure_name – [in] procedure name.  // 输入过程名称。
  - code_type – [in] code type, currently supported PY, SO, CPP, ZIP.  // 输入代码类型，目前支持 PY、SO、CPP、ZIP。
  - procedure_description – [in] procedure description.  // 输入过程描述。
  - read_only – [in] procedure is read only or not.  // 输入过程是否为只读。
  - version – [in] (Optional) the version of procedure.  // 可选，过程版本。
  - graph – [in] (Optional) the graph to query.  // 可选，指定要查询的图。

  **返回**  
  True if it succeeds, false if it fails.  // 如果成功返回真，失败返回假。

- **bool LoadProcedure(std::string &result, const std::vector<std::string> &source_files, const std::string &procedure_type, const std::string &procedure_name, const std::string &code_type, const std::string &procedure_description, bool read_only, const std::string &version = "v1", const std::string &graph = "default")**  
  Load a built-in procedure.  
  加载一个内置过程。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - source_files – [in] the source_file list contain procedure code(only for code_type cpp).  // 输入参数，包含过程代码的源文件列表（仅适用于代码类型为cpp）。
  - procedure_type – [in] the procedure type, currently supported CPP and PY.  // 输入参数，过程类型，目前支持CPP和PY。
  - procedure_name – [in] procedure name.  // 输入参数，过程名称。
  - code_type – [in] code type, currently supported PY, SO, CPP, ZIP.  // 输入参数，代码类型，目前支持PY、SO、CPP、ZIP。
  - procedure_description – [in] procedure description.  // 输入参数，过程描述。
  - read_only – [in] procedure is read only or not.  // 输入参数，过程是否为只读。
  - version – [in] (Optional) the version of procedure.  // 输入参数（可选），过程的版本。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool ListProcedures(std::string &result, const std::string &procedure_type, const std::string &version = "any", const std::string &graph = "default", const std::string &url = "")**  
  List user-defined procedures.  
  列出用户定义的过程。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - procedure_type – [in] (Optional) the procedure type, “” for all procedures, CPP and PY for special type.  // 输入参数（可选），过程类型，""表示所有过程，CPP和PY表示特定类型。
  - version – [in] (Optional) the version of procedure.  // 输入参数（可选），过程的版本。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。
  - url – [in] Node address of calling procedure.  // 输入参数，调用过程的节点地址。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool DeleteProcedure(std::string &result, const std::string &procedure_type, const std::string &procedure_name, const std::string &graph = "default")**  
  Execute a user-defined procedure.  
  执行用户定义的程序。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - procedure_type – [in] the procedure type, currently supported CPP and PY.  // 输入参数，过程类型，目前支持CPP和PY。
  - procedure_name – [in] procedure name.  // 输入参数，过程名称。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool ImportSchemaFromContent(std::string &result, const std::string &schema, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge schema from content string.  
  从内容字符串中导入顶点或边的模式。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - schema – [in] the schema to be imported.  // 输入参数，要导入的模式。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 输入参数（可选），返回格式，true表示json，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 输入参数（可选），最大执行时间，超时将被中断。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool ImportDataFromContent(std::string &result, const std::string &desc, const std::string &data, const std::string &delimiter, bool continue_on_error = false, int thread_nums = 8, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge data from content string.  
  从内容字符串中导入顶点或边的数据。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - desc – [in] data format description.  // 输入参数，数据格式描述。
  - data – [in] the data to be imported.  // 输入参数，要导入的数据。
  - delimiter – [in] data separator.  // 输入参数，数据分隔符。
  - continue_on_error – [in] (Optional) whether to continue when importing data fails.  // 输入参数（可选），导入数据失败时是否继续。
  - thread_nums – [in] (Optional) maximum number of threads.  // 输入参数（可选），最大线程数。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 输入参数（可选），返回格式，true表示json，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 输入参数（可选），最大执行时间，超时将被中断。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool ImportSchemaFromFile(std::string &result, const std::string &schema_file, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge schema from file.  
  从文件导入顶点或边的模式。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - schema_file – [in] the schema_file contain schema.  // 输入参数，包含模式的schema文件。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 输入参数（可选），返回格式，true表示json，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 输入参数（可选），最大执行时间，超时将被中断。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **bool ImportDataFromFile(std::string &result, const std::string &conf_file, const std::string &delimiter, bool continue_on_error = false, int thread_nums = 8, int skip_packages = 0, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge data from file.  
  从文件导入顶点或边的数据。

  **参数**
  - result – [out] The result.  // 输出参数，返回结果。
  - conf_file – [in] data file contain format description and data.  // 输入参数，包含格式描述和数据的数据文件。
  - delimiter – [in] data separator.  // 输入参数，数据分隔符。
  - continue_on_error – [in] (Optional) whether to continue when importing data fails.  // 输入参数（可选），导入数据失败时是否继续。
  - thread_nums – [in] (Optional) maximum number of threads.  // 输入参数（可选），最大线程数。
  - skip_packages – [in] (Optional) skip packages number.  // 输入参数（可选），跳过的包数量。
  - graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。
  - json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  // 输入参数（可选），返回格式，true表示json，否则为二进制格式。
  - timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  // 输入参数（可选），最大执行时间，超时将被中断。

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **int64_t Restore(const std::vector<BackupLogEntry> &requests)**

- **void Logout()**  
  Client log out.  
  客户端登出。

### Private Functions

- **bool IsReadQuery(lgraph::GraphQueryType type, const std::string &query, const std::string &graph)**  
  Determine whether it is a read-only query.  
  确定是否为只读查询。

  **参数**  
  type – [in] inquire query type.  // 输入参数，查询类型。  
  query – [in] inquire statement.  // 输入参数，查询语句。  
  graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。  

  **返回**  
  True if it succeeds, false if it fails.  // 成功返回true，失败返回false。

- **std::shared_ptr<lgraph::RpcClient::RpcSingleClient> GetClient(lgraph::GraphQueryType type, const std::string &cypher, const std::string &graph)**  
  Return rpc client based on whether it is a read-only query.  
  根据是否为只读查询返回rpc客户端。

  **参数**  
  type – [in] inquire query type.  // 输入参数，查询类型。  
  query – [in] inquire statement.  // 输入参数，查询语句。  
  graph – [in] (Optional) the graph to query.  // 输入参数（可选），要查询的图。  

  **返回**  
  Master rpc client if cypher is not read-only, slaver rpc client if cypher is read-only.  // 如果cypher不是只读，返回主rpc客户端；如果cypher是只读，返回从rpc客户端。

- **std::shared_ptr<lgraph::RpcClient::RpcSingleClient> GetClient(bool isReadQuery)**  
  Return rpc client based on whether it is a read-only query.  
  根据是否为只读查询返回rpc客户端。

  **参数**  
  isReadQuery – [in] read query or not.  // 输入参数，是否为只读查询。  

  **返回**  
  Master rpc client if cypher is not read-only, slaver rpc client if cypher is read-only.  // 如果cypher不是只读，返回主rpc客户端；如果cypher是只读，返回从rpc客户端。

- **std::shared_ptr<lgraph::RpcClient::RpcSingleClient> GetClientByNode(const std::string &url)**  
  Get the client according to the node url.  
  根据节点url获取客户端。

  **参数**  
  url – [in] Node address of client connection.  // 输入参数，客户端连接的节点地址。  

  **返回**  
  Rpc client connecting to url.  // 连接到url的Rpc客户端。

- **void RefreshUserDefinedProcedure()**  
  Refresh User-defined Procedure.  
  刷新用户定义的过程。

- **void RefreshBuiltInProcedure()**  
  Refresh Built-in Procedure.  
  刷新内置过程。

- **void RefreshClientPool()**  
  Refresh the client connection pool according to the cluster status.  
  根据集群状态刷新客户端连接池。

- **void LoadBalanceClientPool()**  
  Load balance the client pool.  
  负载均衡客户端池。

- **void RefreshConnection()**  
  Refresh client pool, procedure info.  
  刷新客户端池和过程信息。

- **template<typename F> bool DoubleCheckQuery(F const &f)**  
  If an exception is thrown in the query, refresh the connection and re-execute.  
  如果查询中抛出异常，刷新连接并重新执行。

### Private Members

- **ClientType client_type**  
- **std::string user**  
- **std::string password**  
- **std::shared_ptr<lgraph::RpcClient::RpcSingleClient> base_client**  
- **std::vector<std::string> urls**  
- **std::shared_ptr<lgraph::RpcClient::RpcSingleClient> leader_client**  
- **std::deque<std::shared_ptr<lgraph::RpcClient::RpcSingleClient>> client_pool**  
- **nlohmann::json built_in_procedures = {}**  
- **nlohmann::json user_defined_procedures = {}**  
- **std::vector<std::string> cypher_write_constant**  
- **std::vector<std::string> gql_write_constant**  

### class RpcSingleClient

#### Public Functions

- **RpcSingleClient(const std::string &url, const std::string &user, const std::string &password)**  
  RpcSingleClient Login.  
  RpcSingleClient 登录。

  **参数**  
  url – Login address.  
  url – 登录地址。  
  user – The username.  
  user – 用户名。  
  password – The password.  
  password – 密码。  

- **~RpcSingleClient()**

- **int64_t Restore(const std::vector<BackupLogEntry> &requests)**

- **bool LoadProcedure(std::string &result, const std::vector<std::string> &source_files, const std::string &procedure_type, const std::string &procedure_name, const std::string &code_type, const std::string &procedure_description, bool read_only, const std::string &version = "v1", const std::string &graph = "default")**  
  Load a user-defined procedure.  
  加载用户定义的过程。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  source_files – [in] the source_file list contain procedure code(only for code_type cpp).  
  source_files – [输入] 包含过程代码的源文件列表（仅适用于代码类型 cpp）。  
  procedure_type – [in] the procedure type, currently supported CPP and PY.  
  procedure_type – [输入] 过程类型，目前支持 CPP 和 PY。  
  procedure_name – [in] procedure name.  
  procedure_name – [输入] 过程名称。  
  code_type – [in] code type, currently supported PY, SO, CPP, ZIP.  
  code_type – [输入] 代码类型，目前支持 PY、SO、CPP、ZIP。  
  procedure_description – [in] procedure description.  
  procedure_description – [输入] 过程描述。  
  read_only – [in] procedure is read only or not.  
  read_only – [输入] 过程是否为只读。  
  version – [in] (Optional) the version of procedure.  
  version – [输入] （可选）过程的版本。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool CallProcedure(std::string &result, const std::string &procedure_type, const std::string &procedure_name, const std::string &param, double procedure_time_out = 0.0, bool in_process = false, const std::string &graph = "default", bool json_format = true)**  
  Execute a user-defined procedure.  
  执行用户定义的过程。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  procedure_type – [in] the procedure type, currently supported CPP and PY.  
  procedure_type – [输入] 过程类型，目前支持 CPP 和 PY。  
  procedure_name – [in] procedure name.  
  procedure_name – [输入] 过程名称。  
  param – [in] the execution parameters.  
  param – [输入] 执行参数。  
  procedure_time_out – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  procedure_time_out – [输入] （可选）最大执行时间，超时将被中断。  
  in_process – [in] (Optional) support in future.  
  in_process – [输入] （可选）未来支持。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool ListProcedures(std::string &result, const std::string &procedure_type, const std::string &version = "any", const std::string &graph = "default")**  
  List user-defined procedures.  
  列出用户定义的过程。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  procedure_type – [in] (Optional) the procedure type, “” for all procedures, CPP and PY for special type.  
  procedure_type – [输入] （可选）过程类型，“”表示所有过程，CPP 和 PY 表示特定类型。  
  version – [in] (Optional) the version of procedure.  
  version – [输入] （可选）过程的版本。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool DeleteProcedure(std::string &result, const std::string &procedure_type, const std::string &procedure_name, const std::string &graph = "default")**  
  Execute a user-defined procedure.  
  删除用户定义的程序。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  procedure_type – [in] the procedure type, currently supported CPP and PY.  
  procedure_type – [输入] 过程类型，目前支持 CPP 和 PY。  
  procedure_name – [in] procedure name.  
  procedure_name – [输入] 过程名称。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool ImportSchemaFromFile(std::string &result, const std::string &schema_file, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge schema from file.  
  从文件导入顶点或边的模式。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  schema_file – [in] the schema_file contain schema.  
  schema_file – [输入] 包含模式的模式文件。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool ImportDataFromFile(std::string &result, const std::string &conf_file, const std::string &delimiter, bool continue_on_error = false, int thread_nums = 8, int skip_packages = 0, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge data from file.  
  从文件导入顶点或边的数据。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  conf_file – [in] data file contain format description and data.  
  conf_file – [输入] 数据文件包含格式描述和数据。  
  delimiter – [in] data separator.  
  delimiter – [输入] 数据分隔符。  
  continue_on_error – [in] (Optional) whether to continue when importing data fails.  
  continue_on_error – [输入] （可选）导入数据失败时是否继续。  
  thread_nums – [in] (Optional) maximum number of threads.  
  thread_nums – [输入] （可选）最大线程数。  
  skip_packages – [in] (Optional) skip packages number.  
  skip_packages – [输入] （可选）跳过的包数。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool ImportSchemaFromContent(std::string &result, const std::string &schema, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge schema from content string.  
  从内容字符串导入顶点或边的模式。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  schema – [in] the schema to be imported.  
  schema – [输入] 要导入的模式。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool ImportDataFromContent(std::string &result, const std::string &desc, const std::string &data, const std::string &delimiter, bool continue_on_error = false, int thread_nums = 8, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Import vertex or edge data from content string.  
  从内容字符串导入顶点或边的数据。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  desc – [in] data format description.  
  desc – [输入] 数据格式描述。  
  data – [in] the data to be imported.  
  data – [输入] 要导入的数据。  
  delimiter – [in] data separator.  
  delimiter – [输入] 数据分隔符。  
  continue_on_error – [in] (Optional) whether to continue when importing data fails.  
  continue_on_error – [输入] （可选）导入数据失败时是否继续。  
  thread_nums – [in] (Optional) maximum number of threads.  
  thread_nums – [输入] （可选）最大线程数。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool CallCypher(std::string &result, const std::string &cypher, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Execute a cypher query.  
  执行一个 cypher 查询。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  cypher – [in] inquire statement.  
  cypher – [输入] 查询语句。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **bool CallGql(std::string &result, const std::string &gql, const std::string &graph = "default", bool json_format = true, double timeout = 0)**  
  Execute a gql query.  
  执行一个 gql 查询。

  **参数**  
  result – [out] The result.  
  result – [输出] 结果。  
  gql – [in] inquire statement.  
  gql – [输入] 查询语句。  
  graph – [in] (Optional) the graph to query.  
  graph – [输入] （可选）要查询的图。  
  json_format – [in] (Optional) Returns the format， true is json，Otherwise, binary format.  
  json_format – [输入] （可选）返回格式，真为 json，其他为二进制格式。  
  timeout – [in] (Optional) Maximum execution time, overruns will be interrupted.  
  timeout – [输入] （可选）最大执行时间，超时将被中断。  

  **返回**  
  True if it succeeds, false if it fails.  
  如果成功则返回真，失败则返回假。

- **std::string GetUrl()**  
  Get the url of single client.  
  获取单个客户端的 URL。

  **返回**  
  the url of single client.  
  单个客户端的 URL。

- **void Logout()**  
  Client log out.  
  客户端登出。

### Private Functions

- **LGraphResponse HandleRequest(LGraphRequest *req)**  
- **bool HandleGraphQueryRequest(lgraph::GraphQueryType type, LGraphResponse *res, const std::string &query, const std::string &graph, bool json_format, double timeout)**  
- **std::string GraphQueryResponseExtractor(const GraphQueryResponse &cypher)**  

### Private Members

- **std::string url**  
- **std::string user**  
- **std::string password**  
- **std::string token**  
- **int64_t server_version**  
- **std::shared_ptr<lgraph_rpc::m_channel> channel**  
- **std::shared_ptr<lgraph_rpc::m_controller> cntl**  
- **std::shared_ptr<lgraph_rpc::m_channel_options> options**















## lgraph_traversal

```cpp
namespace lgraph_api
namespace traversal
```

### Functions

- **ParallelVector<size_t> FindVertices(GraphDB &db, Transaction &txn, std::function<bool(VertexIterator&)> filter, bool parallel = false)**  
  Retrieve all vertices passing the specified filter. Note that if the transaction is not read-only, parallel will be ignored (i.e. parallelism will not be available).  
  // 检索所有满足指定过滤条件的顶点。请注意，如果事务不是只读，则并行将被忽略（即无法使用并行）。

  **参数**  
  db – [inout] The GraphDB instance.  
  // db – [输入输出] GraphDB 实例。  
  txn – [inout] The transaction.  
  // txn – [输入输出] 事务。  
  filter – [inout] The user-defined filter function.  
  // filter – [输入输出] 用户定义的过滤函数。  
  parallel – (Optional) Enable parallelism or not.  
  // parallel – （可选）是否启用并行。

  **返回**  
  The corresponding list of vertices.  
  // 返回相应的顶点列表。

- **template<typename VertexData> static ParallelVector<VertexData> ExtractVertexData(GraphDB &db, Transaction &txn, ParallelVector<size_t> &frontier, std::function<void(VertexIterator&, VertexData&)> extract, bool parallel = false)**  
  Extract data from specified vertices. Note that if the transaction is not read-only, parallel will be ignored (i.e. parallelism will not be available).  
  // 从指定的顶点中提取数据。请注意，如果事务不是只读，则并行将被忽略（即无法使用并行）。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  
  // std::runtime_error – 当发生运行时错误条件时引发。

  **模板参数**  
  VertexData – Type of the vertex data.  
  // VertexData – 顶点数据的类型。

  **参数**  
  db – [inout] The database.  
  // db – [输入输出] 数据库。  
  txn – [inout] The transaction.  
  // txn – [输入输出] 事务。  
  frontier – [inout] The vertices to extract data from.  
  // frontier – [输入输出] 要提取数据的顶点。  
  extract – [inout] The user-defined extract function.  
  // extract – [输入输出] 用户定义的提取函数。  
  parallel – (Optional) Enable parallelism or not.  
  // parallel – （可选）是否启用并行。

  **返回**  
  The corresponding extracted data.  
  // 返回相应的提取数据。

- **template<class IT> bool DefaultFilter(IT &it)**  
  The default filter you may use.  
  // 您可以使用的默认过滤器。

  **模板参数**  
  IT – Type of the iterator.  
  // IT – 迭代器的类型。

  **参数**  
  it – [inout] An iterator class.  
  // it – [输入输出] 一个迭代器类。

  **返回**  
  Always true.  
  // 总是返回 true。

- **template<class IT> std::function<bool(IT&)> LabelEquals(const std::string &label)**  
  Function closure filtering by label.  
  // 根据标签进行过滤的函数闭包。

  **模板参数**  
  IT – Type of the iterator.  
  // IT – 迭代器的类型。

  **参数**  
  label – An iterator class.  
  // label – 一个迭代器类。

  **返回**  
  The filter function.  
  // 返回过滤函数。

- **template<class IT> std::function<bool(IT&)> LabelEquals(size_t label_id)**  
  Function closure filtering by label id.  
  // 根据标签 ID 进行过滤的函数闭包。

  **模板参数**  
  IT – Type of the iterator.  
  // IT – 迭代器的类型。

  **参数**  
  label_id – An iterator class.  
  // label_id – 一个迭代器类。

  **返回**  
  The filter function.  
  // 返回过滤函数。

- **bool operator==(const Vertex &lhs, const Vertex &rhs)**  
  // 重载运算符 ==，比较两个 Vertex 对象是否相等。

- **bool operator!=(const Vertex &lhs, const Vertex &rhs)**  
  // 重载运算符 !=，比较两个 Vertex 对象是否不相等。

- **bool operator==(const Edge &lhs, const Edge &rhs)**  
  // 重载运算符 ==，比较两个 Edge 对象是否相等。

- **bool operator!=(const Edge &lhs, const Edge &rhs)**  
  // 重载运算符 !=，比较两个 Edge 对象是否不相等。

### Variables

- `static constexpr size_t MAX_RESULT_SIZE = 1ul << 36`  
  // 最大结果大小的静态常量。

- `static constexpr size_t TRAVERSAL_PARALLEL = 1ul << 0`  
  // 并行遍历的静态常量。

- `static constexpr size_t TRAVERSAL_ALLOW_REVISITS = 1ul << 1`  
  // 允许重新访问的静态常量。

### class Edge

```cpp
#include <lgraph_traversal.h>
```
Represent an edge.  
// 表示一条边。

**Public Functions**

- **Edge(size_t start, uint16_t lid, uint64_t tid, size_t end, size_t eid, bool forward)**  
  Constructor  
  // 构造函数

  **参数**  
  start – The start.  
  // start – 起始位置。  
  lid – The lid.  
  // lid – 标签 ID。  
  tid – The tid.  
  // tid – 时间 ID。  
  end – The end.  
  // end – 结束位置。  
  eid – The eid.  
  // eid – 边 ID。  
  forward – True to forward.  
  // forward – 如果为 true，则表示为正向。

- **Edge(const Edge &rhs) = default**  
  Copy constructor  
  // 拷贝构造函数

  **参数**  
  rhs – The right hand side.  
  // rhs – 右侧对象。

- **Vertex GetStartVertex() const**  
  Get the start vertex of this edge.  
  // 获取此边的起始顶点。

  **返回**  
  The start vertex.  
  // 返回起始顶点。

- **Vertex GetEndVertex() const**  
  Get the end vertex of this edge.  
  // 获取此边的结束顶点。

  **返回**  
  The end vertex.  
  // 返回结束顶点。

- **uint16_t GetLabelId() const**  
  Get the label ID.  
  // 获取标签 ID。

  **返回**  
  The label ID.  
  // 返回标签 ID。

- **uint64_t GetTemporalId() const**  
  Get the Temporal ID.  
  // 获取时间 ID。

  **返回**  
  The temporal ID.  
  // 返回时间 ID。

- **size_t GetEdgeId() const**  
  Get the edge ID.  
  // 获取边 ID。

  **返回**  
  The edge ID.  
  // 返回边 ID。

- **bool IsForward() const**  
  Get the direction of this edge.  
  // 获取此边的方向。

  **返回**  
  true : forward; false: backward.  
  // 返回 true 表示正向，false 表示反向。

- **Vertex GetSrcVertex() const**  
  Get the source vertex of this edge.  
  // 获取此边的源顶点。

  **返回**  
  The source vertex.  
  // 返回源顶点。

- **Vertex GetDstVertex() const**  
  Get the destination vertex of this edge.  
  // 获取此边的目标顶点。

  **返回**  
  The destination vertex.  
  // 返回目标顶点。

**Private Members**

- size_t start_  
  // 起始位置。

- size_t end_  
  // 结束位置。

- size_t eid_  
  // 边 ID。

- uint16_t lid_  
  // 标签 ID。

- int64_t tid_  
  // 时间 ID。

- bool forward_  
  // 方向标志。

**Friends**

- friend class Path  
  // 友元类 Path  
- friend class IteratorHelper  
  // 友元类 IteratorHelper  
- friend class PathTraversal  
  // 友元类 PathTraversal  

### class FrontierTraversal

```cpp
#include <lgraph_traversal.h>
```
FrontierTraversal provides the most common way to traverse graphs. You can start from a single vertex or a set of vertices (known as the initial frontier), and expand them frontier by frontier, each time visiting neighboring vertices of the current frontier and make those matching specified conditions the new frontier. One powerful feature of FrontierTraversal is that the traversal can be performed in parallel, accelerating those deep queries significantly.  
// FrontierTraversal 提供了遍历图的最常用方式。您可以从单个顶点或一个顶点集合（称为初始前沿）开始，然后逐渐扩展每个前沿，每次访问当前前沿的邻居顶点，并使符合指定条件的顶点成为新的前沿。FrontierTraversal 的一个强大特性是遍历可以并行执行，从而显著加速深度查询。

**Public Functions**

- **FrontierTraversal(GraphDB &db, Transaction &txn, size_t flags = 0, size_t capacity = MAX_RESULT_SIZE)**  
  Construct the FrontierTraversal object. Note that the transaction must be read-only if you want to perform the traversals in parallel (i.e. TRAVERSAL_PARALLEL is specified in flags). Be careful when TRAVERSAL_ALLOW_REVISITS is used, as each vertex may be visited more than once, making the result set huge.  
  // 构造 FrontierTraversal 对象。请注意，如果您希望并行执行遍历，则事务必须是只读的（即在标志中指定了 TRAVERSAL_PARALLEL）。使用 TRAVERSAL_ALLOW_REVISITS 时要小心，因为每个顶点可能会被访问多次，从而使结果集变得庞大。

  **参数**  
  db – [inout] The GraphDB instance.  
  // db – [输入输出] GraphDB 实例。  
  txn – [inout] The transaction.  
  // txn – [输入输出] 事务。  
  flags – (Optional) The options used during traversals.  
  // flags – （可选）遍历时使用的选项。

- **ParallelVector<size_t> &GetFrontier()**  
  Retrieve the current (i.e. latest) frontier.  
  // 获取当前（即最新）的前沿。

  **返回**  
  The frontier.  
  // 返回前沿。

- **void SetFrontier(size_t root_vid)**  
  Set the (initial) frontier to contain a single vertex.  
  // 设置（初始）前沿为单个顶点。

  **参数**  
  root_vid – The identifer for the starting vertex.  
  // root_vid – 起始顶点的标识符。

- **void SetFrontier(ParallelVector<size_t> &root_vids)**  
  Set the (initial) frontier to contain a set of vertices.  
  // 设置（初始）前沿为一组顶点。

  **参数**  
  root_vids – [inout] The starting vertex identifiers.  
  // root_vids – [输入输出] 起始顶点的标识符。

- **void SetFrontier(std::function<bool(VertexIterator&)> root_vertex_filter)**  
  Set the (initial) frontier by using a filter function. Each vertex will be checked against the specified filter.  
  // 使用过滤函数设置（初始）前沿。每个顶点将根据指定的过滤器进行检查。

  **参数**  
  root_vertex_filter – [inout] The filter function.  
  // root_vertex_filter – [输入输出] 过滤函数。

- **void ExpandOutEdges(std::function<bool(OutEdgeIterator&)> out_edge_filter = nullptr, std::function<bool(VertexIterator&)> out_neighbour_filter = nullptr)**  
  Expand the current frontier through outgoing edges using filters. Note that the default value for the two filters (nullptr) means all the expansions of this level should succeed and enables some optimizations.  
  // 通过使用过滤器通过出边扩展当前前沿。请注意，两个过滤器的默认值（nullptr）意味着该级别的所有扩展应该成功，并启用一些优化。

  **参数**  
  out_edge_filter – [inout] (Optional) The filter for an outgoing edge.  
  // out_edge_filter – [输入输出] （可选）出边的过滤器。  
  out_neighbour_filter – [inout] (Optional) The filter for an destination vertex.  
  // out_neighbour_filter – [输入输出] （可选）目标顶点的过滤器。

- **void ExpandInEdges(std::function<bool(InEdgeIterator&)> in_edge_filter = nullptr, std::function<bool(VertexIterator&)> in_neighbour_filter = nullptr)**  
  Expand the current frontier through incoming edges using filters. Note that the default value for the two filters (nullptr) means all the expansions of this level should succeed and enables some optimizations.  
  // 通过使用过滤器通过入边扩展当前前沿。请注意，两个过滤器的默认值（nullptr）意味着该级别的所有扩展应该成功，并启用一些优化。

  **参数**  
  in_edge_filter – [inout] (Optional) The filter for an incoming edge.  
  // in_edge_filter – [输入输出] （可选）入边的过滤器。  
  in_neighbour_filter – [inout] (Optional) The filter for an source vertex.  
  // in_neighbour_filter – [输入输出] （可选）源顶点的过滤器。

- **void ExpandEdges(std::function<bool(OutEdgeIterator&)> out_edge_filter = nullptr, std::function<bool(InEdgeIterator&)> in_edge_filter = nullptr, std::function<bool(VertexIterator&)> out_neighbour_filter = nullptr, std::function<bool(VertexIterator&)> in_neighbour_filter = nullptr)**  
  Expand the current frontier through both directions using filters. Note that the default value for the two filters (nullptr) means all the expansions of this level should succeed and enables some optimizations.  
  // 通过使用过滤器在两个方向上扩展当前前沿。请注意，两个过滤器的默认值（nullptr）意味着该级别的所有扩展应该成功，并启用一些优化。

  **参数**  
  out_edge_filter – [inout] (Optional) The filter for an outgoing edge.  
  // out_edge_filter – [输入输出] （可选）出边的过滤器。  
  in_edge_filter – [inout] (Optional) The filter for an incoming edge.  
  // in_edge_filter – [输入输出] （可选）入边的过滤器。  
  out_neighbour_filter – [inout] (Optional) The filter for an destination vertex.  
  // out_neighbour_filter – [输入输出] （可选）目标顶点的过滤器。  
  in_neighbour_filter – [inout] (Optional) The filter for an source vertex.  
  // in_neighbour_filter – [输入输出] （可选）源顶点的过滤器。

- **void Reset()**  
  Reset the traversal.  
  // 重置遍历。

- **void ResetVisited()**  
  Reset only the visited flags.  
  // 仅重置已访问的标志。

**Private Members**

- GraphDB &db_  
  // GraphDB 引用。

- Transaction &txn_  
  // 事务引用。

- size_t flags_  
  // 标志。

- size_t num_vertices_  
  // 顶点数量。

- ParallelVector<Path> curr_frontier_  
  // 当前前沿。

- ParallelVector<Path> next_frontier_  
  // 下一个前沿。

### class Vertex

```cpp
#include <lgraph_traversal.h>
```
Represent a vertex.  
// 表示一个顶点。

#### Public Functions

- **explicit Vertex(size_t vid)**  
  Constructor  
  // 构造函数

  **参数**  
  vid – The vid.  
  // vid – 顶点 ID。

- **Vertex(const Vertex &rhs) = default**  
  Copy constructor  
  // 拷贝构造函数

  **参数**  
  rhs – The right hand side.  
  // rhs – 右侧对象。

- **size_t GetId() const**  
  Get the Id of this vertex.  
  // 获取此顶点的 ID。

  **返回**  
  The Id.  
  // 返回 ID。

#### Private Members

- size_t vid_  
  // 顶点 ID。  

#### Friends

- friend class Path  
  // 友元类 Path  
- friend class IteratorHelper  
  // 友元类 IteratorHelper  
- friend class PathTraversal  
  // 友元类 PathTraversal












  


## lgraph_txn

```cpp
namespace lgraph
namespace lgraph_api
class Transaction
#include <lgraph_txn.h>
```

TuGraph operations happen in transactions. A transaction is a sequence of operations that is carried out atomically on the GraphDB. TuGraph transactions provide full ACID guarantees.
// TuGraph 操作在交易中进行。一个交易是一系列在 GraphDB 上原子执行的操作。TuGraph 交易提供完全的 ACID 保证。

Transactions are created using `GraphDB::CreateReadTxn()` and `GraphDB::CreateWriteTxn()`. A read transaction can only perform read operations, otherwise an exception is thrown. A write transaction can perform reads as well as writes. There are performance differences between read and write operations. So if you only need read in a transaction, you should create a read transaction.
// 交易是通过 `GraphDB::CreateReadTxn()` 和 `GraphDB::CreateWriteTxn()` 创建的。读交易只能执行读取操作，否则会抛出异常。写交易可以执行读取和写入操作。读取和写入操作之间存在性能差异。因此，如果您只需要在交易中读取，您应该创建一个读取交易。

Each transaction must be used in one thread only, and they should not be passed from one thread to another unless it is a forked transaction.
// 每个交易必须只在一个线程中使用，并且它们不应从一个线程传递到另一个线程，除非是一个派生交易。

Read transactions can be forked. The new copy of the transaction will have the same view as the forked one, and it can be used in a separate thread. By forking from one read transaction and using the forked copies in different threads, we can parallelize the execution of specific operations. For example, you can implement a parallel BFS with this capability. Also, you can dump a snapshot of the whole graph using.
// 读交易可以被派生。新副本的交易将与被派生的交易具有相同的视图，并且可以在不同的线程中使用。通过从一个读取交易派生并在不同线程中使用派生的副本，我们可以并行化特定操作的执行。例如，您可以利用此能力实现并行 BFS。同时，您可以使用它导出整个图的快照。

### Public Functions

```cpp
Transaction(Transaction &&rhs) = default
Transaction &operator=(Transaction &&rhs) = default
Transaction(const Transaction&) = delete
Transaction &operator=(const Transaction&) = delete
```

```cpp
void Commit()
```
Commits this transaction. Note that optimistic write transactions may fail to commit (an TxnConflict would be thrown).
// 提交此交易。请注意，乐观写交易可能会无法提交（将抛出 TxnConflict）。

```cpp
void Abort()
```
Aborts this transaction.
// 中止此交易。

```cpp
bool IsValid() const
```
Query if this transaction is valid. Transaction becomes invalid after calling Abort() or Commit(). Operations on invalid transaction yield exceptions.
// 查询此交易是否有效。交易在调用 Abort() 或 Commit() 后变为无效。在无效交易上执行操作将导致异常。

返回  
True if valid, false if not.
// 如果有效返回 true，否则返回 false。

```cpp
bool IsReadOnly() const
```
Query if this txn is read only.
// 查询此交易是否为只读。

返回  
True if read only, false if not.
// 如果是只读返回 true，否则返回 false。

```cpp
const std::shared_ptr<lgraph::Transaction> GetTxn()
```
Get Transaction.
// 获取交易。

返回  
Transaction.
// 交易。

```cpp
VertexIterator GetVertexIterator()
```
Get a vertex iterator pointing to the first vertex. If there is no vertex, the iterator is invalid.
// 获取指向第一个顶点的顶点迭代器。如果没有顶点，则迭代器无效。

返回  
The vertex iterator.
// 顶点迭代器。

```cpp
VertexIterator GetVertexIterator(int64_t vid, bool nearest = false)
```
Gets a vertex iterator pointing to the Vertex with vid. If the vertex does not exist, the iterator is invalid. If nearest==true, the iterator points to the first vertex sorted by vid, with id>=vid.
// 获取指向具有 vid 的顶点的顶点迭代器。如果该顶点不存在，则迭代器无效。如果 nearest==true，迭代器指向按 vid 排序的第一个顶点，具有 id>=vid。

参数  
vid – The vid.  
// vid – 顶点 ID。  
nearest – (Optional) True to point to the nearest vertex sorted by vid.
// nearest – （可选）如果为 true，则指向按 vid 排序的最近顶点。

返回  
The vertex iterator.
// 顶点迭代器。

```cpp
OutEdgeIterator GetOutEdgeIterator(const EdgeUid &euid, bool nearest = false)
```
Gets an out edge iterator pointing to the edge specified by euid. If nearest==true, and the specified edge does not exist, return the first edge that sorts after the specified one.
// 获取指向由 euid 指定的边的出边迭代器。如果 nearest==true，并且指定的边不存在，则返回在指定边之后排序的第一条边。

参数  
euid – Edge Unique Id.  
// euid – 边的唯一 ID。  
nearest – (Optional) If true, get the first edge that sorts after the specified one if the specified one does not exist.
// nearest – （可选）如果为 true，则在指定边不存在时，获取在指定边之后排序的第一条边。

返回  
The out edge iterator.
// 出边迭代器。

```cpp
OutEdgeIterator GetOutEdgeIterator(const int64_t src, const int64_t dst, const int16_t lid)
```

```cpp
InEdgeIterator GetInEdgeIterator(const EdgeUid &euid, bool nearest = false)
```
Gets an in edge iterator pointing to the edge specified by euid. If nearest==true, and the specified edge does not exist, return the first edge that sorts after the specified one.
// 获取指向由 euid 指定的边的入边迭代器。如果 nearest==true，并且指定的边不存在，则返回在指定边之后排序的第一条边。

参数  
euid – Edge Unique Id.  
// euid – 边的唯一 ID。  
nearest – (Optional) If true, get the first edge that sorts after the specified one if the specified one does not exist.
// nearest – （可选）如果为 true，则在指定边不存在时，获取在指定边之后排序的第一条边。

返回  
The out edge iterator.
//出边迭代器。

```cpp
InEdgeIterator GetInEdgeIterator(const int64_t src, const int64_t dst, const int16_t lid)
```

```cpp
size_t GetNumVertexLabels()
```
Gets number of vertex labels.
// 获取顶点标签的数量。

返回  
The number of vertex labels.
// 顶点标签的数量。

```cpp
size_t GetNumEdgeLabels()
```
Gets number of edge labels.
// 获取边标签的数量。

返回  
The number of edge labels.
// 边标签的数量。

```cpp
std::vector<std::string> ListVertexLabels()
```
Lists all vertex labels.
// 列出所有顶点标签。

返回  
Label names.
// 标签名称。

```cpp
std::vector<std::string> ListEdgeLabels()
```
List all edge labels.
// 列出所有边标签。

返回  
Label names.
// 标签名称。

```cpp
size_t GetVertexLabelId(const std::string &label)
```
Gets vertex label id corresponding to the label name.
// 获取与标签名称对应的顶点标签 ID。

参数  
label – The label name.
// label – 标签名称。

返回  
The label id.
// 标签 ID。

```cpp
size_t GetEdgeLabelId(const std::string &label)
```
Gets edge label id corresponding to the label name.
// 获取与标签名称对应的边标签 ID。

参数  
label – The label.
// label – 标签。

返回  
The edge label id.
// 边标签 ID。

```cpp
std::vector<FieldSpec> GetVertexSchema(const std::string &label)
```
Gets edge schema definition corresponding to the vertex label.
// 获取与顶点标签对应的边架构定义。

参数  
label – The label.
// label – 标签。

返回  
The schema.
// 架构。

```cpp
std::vector<FieldSpec> GetEdgeSchema(const std::string &label)
```
Gets edge schema definition corresponding to the edge label.
// 获取与边标签对应的边架构定义。

参数  
label – The label.
// label – 标签。

返回  
The edge schema.
// 边架构。

```cpp
size_t GetVertexFieldId(size_t label_id, const std::string &field_name)
```
Gets vertex field id.
// 获取顶点字段 ID。

参数  
label_id – Identifier for the label.  
// label_id – 标签的标识符。  
field_name – Field name.
// field_name – 字段名称。

返回  
The vertex field identifiers.
// 顶点字段标识符。

```cpp
std::vector<size_t> GetVertexFieldIds(size_t label_id, const std::vector<std::string> &field_names)
```
Gets vertex field ids.
// 获取顶点字段 ID。

参数  
label_id – Identifier for the label.  
// label_id – 标签的标识符。  
field_names – Field names.
// field_names – 字段名称。

返回  
The vertex field identifiers.
// 顶点字段标识符。

```cpp
size_t GetEdgeFieldId(size_t label_id, const std::string &field_name)
```
Gets edge field id.
// 获取边字段 ID。

参数  
label_id – Identifier for the label.  
// label_id – 标签的标识符。  
field_name – Field name.
// field_name – 字段名称。

返回  
The edge field identifier.
// 边字段标识符。

```cpp
std::vector<size_t> GetEdgeFieldIds(size_t label_id, const std::vector<std::string> &field_names)
```
Gets edge field ids.
// 获取边字段 ID。

参数  
label_id – Identifier for the label.  
// label_id – 标签的标识符。  
field_names – Field names.
// field_names – 字段名称。

返回  
The edge field identifier.
// 边字段标识符。

```cpp
int64_t AddVertex(const std::string &label_name, const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Adds a vertex. All non-nullable fields must be specified. VertexIndex is also updated. If a unique_id is indexed for the vertex, and the same unique_id exists, an exception is thrown.
// 添加一个顶点。必须指定所有非空字段。VertexIndex 也会更新。如果为顶点索引了 unique_id，并且相同的 unique_id 已存在，则会抛出异常。

参数  
label_name – Name of the label.  
// label_name – 标签的名称。  
field_names – List of names of the fields.  
// field_names – 字段名称的列表。  
field_value_strings – The field values in string representation.
// field_value_strings – 字段值的字符串表示形式。

返回  
Vertex id of the new vertex.
// 新顶点的 ID。

```cpp
int64_t AddVertex(const std::string &label_name, const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Adds a vertex. All non-nullable fields must be specified. VertexIndex is also updated. If a unique_id is indexed for the vertex, and the same unique_id exists, an exception is thrown.
// 添加一个顶点。必须指定所有非空字段。VertexIndex 也会更新。如果为顶点索引了 unique_id，并且相同的 unique_id 已存在，则会抛出异常。

参数  
label_name – Name of the label.  
// label_name – 标签的名称。  
field_names – List of names of the fields.  
// field_names – 字段名称的列表。  
field_values – The field values.
// field_values – 字段值。

返回  
Vertex id of the new vertex.
// 新顶点的 ID。

```cpp
int64_t AddVertex(size_t label_id, const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Adds a vertex. All non-nullable fields must be specified. VertexIndex is also updated. If a unique_id is indexed for the vertex, and the same unique_id exists, an exception is thrown.  
添加一个顶点。必须指定所有不能为空的字段。顶点索引也会被更新。如果顶点的 unique_id 被索引，并且存在相同的 unique_id，将抛出异常。

参数  
label_id – Label id.  
field_ids – List of field ids.  
field_values – The field values.  
标签 id – 标签 ID。  
field_ids – 字段 ID 列表。  
field_values – 字段值。

返回  
Vertex id of the new vertex.  
新顶点的顶点 ID。

```cpp
int UpsertVertex(size_t label_id, size_t primary_pos, const std::vector<size_t> &unique_pos, const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Upsert a vertex.  
插入或更新一个顶点。

参数  
label_id – Label id.  
primary_pos – The location of the primary field in field_ids.  
unique_pos – The locations of the unique index field in field_ids, can be empty.  
field_ids – List of field ids.  
field_values – The field values.  
标签 id – 标签 ID。  
primary_pos – 字段 ID 中主字段的位置。  
unique_pos – 字段 ID 中唯一索引字段的位置， 可以为空。  
field_ids – 字段 ID 列表。  
field_values – 字段值。

返回  
0: nothing happened because of index conflict 1: the vertex is inserted 2: the vertex is updated.  
0：由于索引冲突而没有发生任何事情 1：顶点已插入 2：顶点已更新。

```cpp
EdgeUid AddEdge(int64_t src, int64_t dst, const std::string &label, const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Adds an edge. All non-nullable fields must be specified. An exception is thrown if src or dst does not exist.  
添加一条边。必须指定所有不能为空的字段。如果 src 或 dst 不存在，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label – The label name.  
field_names – List of field names.  
field_value_strings – List of field values in string representation.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label – 标签名称。  
field_names – 字段名称列表。  
field_value_strings – 字段值字符串表示的列表。

返回  
EdgeUid of the new edge.  
新边的 EdgeUid。

```cpp
EdgeUid AddEdge(int64_t src, int64_t dst, const std::string &label, const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Adds an edge. All non-nullable fields must be specified. An exception is thrown if src or dst does not exist.  
添加一条边。必须指定所有不能为空的字段。如果 src 或 dst 不存在，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label – The label name.  
field_names – List of field names.  
field_values – List of field values.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label – 标签名称。  
field_names – 字段名称列表。  
field_values – 字段值的列表。

返回  
EdgeUid of the new edge.  
新边的 EdgeUid。

```cpp
EdgeUid AddEdge(int64_t src, int64_t dst, size_t label_id, const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Adds an edge. All non-nullable fields must be specified. An exception is thrown if src or dst does not exist.  
添加一条边。必须指定所有不能为空的字段。如果 src 或 dst 不存在，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label_id – The label id.  
field_ids – List of field ids.  
field_values – List of field values.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label_id – 标签 ID。  
field_ids – 字段 ID 列表。  
field_values – 字段值的列表。

返回  
EdgeUid of the new edge.  
新边的 EdgeUid。

```cpp
bool UpsertEdge(int64_t src, int64_t dst, const std::string &label, const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Upsert edge. If there is no src->dst edge, insert it. Otherwise, try to update the edge’s property. If the edge exists and the label differs from specified label, an exception is thrown.  
插入或更新边。如果没有 src->dst 的边，则插入它。否则，尝试更新边的属性。如果边存在并且标签与指定标签不同，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label – The label name.  
field_names – List of field names.  
field_value_strings – List of field values in string representation.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label – 标签名称。  
field_names – 字段名称列表。  
field_value_strings – 字段值字符串表示的列表。

返回  
True if the edge is inserted, false if the edge is updated.  
如果边已存在，则返回False；如果新建了边，则返回True。

```cpp
bool UpsertEdge(int64_t src, int64_t dst, const std::string &label, const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Upsert edge. If there is no src->dst edge, insert it. Otherwise, try to update the edge’s property. If the edge exists and the label differs from specified label, an exception is thrown.  
插入或更新边。如果没有 src->dst 的边，则插入它。否则，尝试更新边的属性。如果边存在并且标签与指定标签不同，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label – The label name.  
field_names – List of field names.  
field_values – List of field values.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label – 标签名称。  
field_names – 字段名称列表。  
field_values – 字段值的列表。

返回  
True if the edge is inserted, false if the edge is updated.  
如果边已存在，则返回False；如果新建了边，则返回True。

```cpp
bool UpsertEdge(int64_t src, int64_t dst, size_t label_id, const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Upsert edge. If there is no src->dst edge, insert it. Otherwise, try to update the edge’s property. If the edge exists and the label differs from specified label, an exception is thrown.  
插入或更新边。如果没有 src->dst 的边，则插入它。否则，尝试更新边的属性。如果边存在并且标签与指定标签不同，将抛出异常。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label_id – The label id.  
field_ids – List of field ids.  
field_values – List of field values.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label_id – 标签 ID。  
field_ids – 字段 ID 列表。  
field_values – 字段值的列表。

返回  
True if the edge is inserted, false if the edge is updated.  
如果边已存在，则返回False；如果新建了边，则返回True。

```cpp
int UpsertEdge(int64_t src, int64_t dst, size_t label_id, const std::vector<size_t> &unique_pos, const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values, std::optional<size_t> pair_unique_pos)
```
Upsert edge. If there is no src->dst edge, insert it. Otherwise, try to update the edge’s property.  
插入或更新边。如果没有 src->dst 的边，则插入它。否则，尝试更新边的属性。

参数  
src – Source vertex id.  
dst – Destination vertex id.  
label_id – The label id.  
unique_pos – The locations of the unique index field in field_ids, can be empty.  
field_ids – List of field ids.  
field_values – List of field values.  
src – 源顶点 ID。  
dst – 目标顶点 ID。  
label_id – 标签 ID。  
unique_pos – 字段 ID 中唯一索引字段的位置，可以为空。  
field_ids – 字段 ID 列表。  
field_values – 字段值的列表。

返回  
0: nothing happened because of index conflict 1: the vertex is inserted 2: the vertex is updated  
0：由于索引冲突而没有发生任何事情 1：顶点已插入 2：顶点已更新

```cpp
std::vector<IndexSpec> ListVertexIndexes()
```
List indexes  
列出索引

返回  
A vector of vertex index specs.  
一个顶点索引规格的向量。

```cpp
std::vector<CompositeIndexSpec> ListVertexCompositeIndexes()
```
List indexes  
列出索引

返回  
A vector of vertex composite index specs.  
一个顶点组合索引规格的向量。

```cpp
std::vector<IndexSpec> ListEdgeIndexes()
```
List indexes  
列出索引

返回  
A vector of edge index specs.  
一个边索引规格的向量。

```cpp
VertexIndexIterator GetVertexIndexIterator(size_t label_id, size_t field_id, const FieldData &key_start, const FieldData &key_end)
```
Gets vertex index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all vertexes that has field value v.  
获取顶点索引迭代器。迭代器包含字段值 [key_start, key_end]。因此，如果 key_start=key_end=v，将返回一个指向所有具有字段值 v 的顶点的迭代器。

参数  
label_id – The label id.  
field_id – The field id.  
key_start – The key start.  
key_end – The key end, inclusive.  
label_id – 标签 ID。  
field_id – 字段 ID。  
key_start – 起始键。  
key_end – 结束键（包括）。

返回  
The index iterator.  
索引迭代器。

```cpp
VertexCompositeIndexIterator GetVertexCompositeIndexIterator(size_t label_id, const std::vector<size_t> &field_id, const std::vector<FieldData> &key_start, const std::vector<FieldData> &key_end)
```
```cpp
EdgeIndexIterator GetEdgeIndexIterator(size_t label_id, size_t field_id, const FieldData &key_start, const FieldData &key_end)
```
Gets edge index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all edges that has field value v.  
获取边索引迭代器。迭代器包含字段值 [key_start, key_end]。因此，如果 key_start=key_end=v，将返回一个指向所有具有字段值 v 的边的迭代器。

参数  
label_id – The label id.  
field_id – The field id.  
key_start – The key start.  
key_end – The key end, inclusive.  
label_id – 标签 ID。  
field_id – 字段 ID。  
key_start – 起始键。  
key_end – 结束键（包括）。

返回  
The index iterator.  
索引迭代器。

```cpp
EdgeIndexIterator GetEdgePairUniqueIndexIterator(size_t label_id, size_t field_id, int64_t src_vid, int64_t dst_vid, const FieldData &key_start, const FieldData &key_end)
```
```cpp
VertexIndexIterator GetVertexIndexIterator(const std::string &label, const std::string &field, const FieldData &key_start, const FieldData &key_end)
```
Gets vertex index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all vertexes that has field value v.  
获取顶点索引迭代器。迭代器包含字段值 [key_start, key_end]。因此，如果 key_start=key_end=v，将返回一个指向所有具有字段值 v 的顶点的迭代器。

参数  
label – The label.  
field – The field.  
key_start – The key start.  
key_end – The key end, inclusive.  
label – 标签。  
field – 字段。  
key_start – 起始键。  
key_end – 结束键（包括）。

返回  
The index iterator.  
索引迭代器。

```cpp
VertexCompositeIndexIterator GetVertexCompositeIndexIterator(const std::string &label, const std::vector<std::string> &field, const std::vector<FieldData> &key_start, const std::vector<FieldData> &key_end)
```
```cpp
EdgeIndexIterator GetEdgeIndexIterator(const std::string &label, const std::string &field, const FieldData &key_start, const FieldData &key_end)
```
Gets index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all edges that has field value v.  
获取索引迭代器。迭代器包含字段值 [key_start, key_end]。因此，如果 key_start=key_end=v，将返回一个指向所有具有字段值 v 的边的迭代器。

参数  
label – The label.  
field – The field.  
key_start – The key start.  
key_end – The key end, inclusive.  
label – 标签。  
field – 字段。  
key_start – 起始键。  
key_end – 结束键（包括）。

返回  
The index iterator.  
索引迭代器。

```cpp
VertexIndexIterator GetVertexIndexIterator(const std::string &label, const std::string &field, const std::string &key_start, const std::string &key_end)
```
Gets index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all vertexes that has field value v.  
获取索引迭代器。迭代器具有字段值 [key_start, key_end]。因此，当 key_start=key_end=v 时，它将返回一个指向所有字段值为 v 的顶点的迭代器。

参数  
label – The label.  标签名称  
field – The field.  字段名称  
key_start – The key start.  键的起始值  
key_end – The key end.  键的结束值  

返回  
The index iterator.  索引迭代器  

```cpp
VertexCompositeIndexIterator GetVertexCompositeIndexIterator(const std::string &label, const std::vector<std::string> &field, const std::vector<std::string> &key_start, const std::vector<std::string> &key_end)
```

```cpp
EdgeIndexIterator GetEdgeIndexIterator(const std::string &label, const std::string &field, const std::string &key_start, const std::string &key_end)
```
Gets index iterator. The iterator has field value [key_start, key_end]. So key_start=key_end=v returns an iterator pointing to all edges that has field value v.  
获取索引迭代器。迭代器具有字段值 [key_start, key_end]。因此，当 key_start=key_end=v 时，它将返回一个指向所有字段值为 v 的边的迭代器。

参数  
label – The label.  标签名称  
field – The field.  字段名称  
key_start – The key start.  键的起始值  
key_end – The key end.  键的结束值  

返回  
The index iterator.  索引迭代器  

```cpp
bool IsVertexIndexed(const std::string &label, const std::string &field)
```
Query if index is ready for use. This should be used only to decide whether to use an index. To wait for an index to be ready, use lgraphDB::WaitIndexReady().  
查询索引是否准备好使用。这只能用于决定是否使用索引。要等待索引准备就绪，请使用 lgraphDB::WaitIndexReady()。

VertexIndex building is async, especially when added for a (label, field) that already has a lot of vertices. This function tells us if the index building is finished.  
VertexIndex 的构建是异步的，尤其是在已经有很多顶点的 (label, field) 上添加时。此函数告诉我们索引构建是否完成。

DO NOT wait for index building in a transaction. Write transactions block other write transactions, so blocking in a write transaction is always a bad idea. And long-living read transactions interfere with GC, making the DB grow unexpectedly.  
请勿在事务中等待索引构建。写事务会阻止其他写事务，因此在写事务中阻塞始终是个坏主意。长期存在的读事务会干扰 GC，导致数据库意外增长。

参数  
label – The label.  标签名称  
field – The field.  字段名称  

返回  
True if index ready, false if not.  如果索引准备好，则返回 true，否则返回 false。  

```cpp
bool IsEdgeIndexed(const std::string &label, const std::string &field)
```
Query if index is ready for use. This should be used only to decide whether to use an index. To wait for an index to be ready, use lgraphDB::WaitIndexReady().  
查询索引是否准备好使用。这只能用于决定是否使用索引。要等待索引准备就绪，请使用 lgraphDB::WaitIndexReady()。

VertexIndex building is async, especially when added for a (label, field) that already has a lot of edges. This function tells us if the index building is finished.  
VertexIndex 的构建是异步的，尤其是在已经有很多边的 (label, field) 上添加时。此函数告诉我们索引构建是否完成。

DO NOT wait for index building in a transaction. Write transactions block other write transactions, so blocking in a write transaction is always a bad idea. And long-living read transactions interfere with GC, making the DB grow unexpectedly.  
请勿在事务中等待索引构建。写事务会阻止其他写事务，因此在写事务中阻塞始终是个坏主意。长期存在的读事务会干扰 GC，导致数据库意外增长。

参数  
label – The label.  标签名称  
field – The field.  字段名称  

返回  
True if index ready, false if not.  如果索引准备好，则返回 true，否则返回 false。  

```cpp
VertexIterator GetVertexByUniqueIndex(const std::string &label_name, const std::string &field_name, const std::string &field_value_string)
```
Gets vertex by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取顶点。如果没有这样的顶点，抛出异常。

参数  
label_name – Name of the label.  标签的名称  
field_name – Name of the field.  字段的名称  
field_value_string – The field value string.  字段值字符串  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
VertexIterator GetVertexByUniqueCompositeIndex(const std::string &label_name, const std::vector<std::string> &field_name, const std::vector<std::string> &field_value_string)
```

```cpp
OutEdgeIterator GetEdgeByUniqueIndex(const std::string &label_name, const std::string &field_name, const std::string &field_value_string)
```
Gets edge by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取边。如果没有这样的顶点，抛出异常。

参数  
label_name – Name of the label.  标签的名称  
field_name – Name of the field.  字段的名称  
field_value_string – The field value string.  字段值字符串  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
VertexIterator GetVertexByUniqueIndex(const std::string &label_name, const std::string &field_name, const FieldData &field_value)
```
Gets vertex by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取顶点。如果没有这样的顶点，抛出异常。

参数  
label_name – Name of the label.  标签的名称  
field_name – Name of the field.  字段的名称  
field_value – The field value.  字段值  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
VertexIterator GetVertexByUniqueCompositeIndex(const std::string &label_name, const std::vector<std::string> &field_name, const std::vector<FieldData> &field_value)
```

```cpp
OutEdgeIterator GetEdgeByUniqueIndex(const std::string &label_name, const std::string &field_name, const FieldData &field_value)
```
Gets edge by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取边。如果没有这样的顶点，抛出异常。

参数  
label_name – Name of the label.  标签的名称  
field_name – Name of the field.  字段的名称  
field_value – The field value.  字段值  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
VertexIterator GetVertexByUniqueIndex(size_t label_id, size_t field_id, const FieldData &field_value)
```
Gets vertex by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取顶点。如果没有这样的顶点，抛出异常。

参数  
label_id – Identifier for the label.  标签的标识符  
field_id – Identifier for the field.  字段的标识符  
field_value – The field value.  字段值  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
VertexIterator GetVertexByUniqueCompositeIndex(size_t label_id, const std::vector<size_t> &field_id, const std::vector<FieldData> &field_value)
```

```cpp
OutEdgeIterator GetEdgeByUniqueIndex(size_t label_id, size_t field_id, const FieldData &field_value)
```
Gets edge by unique index. Throws exception if there is no such vertex.  
通过唯一索引获取边。如果没有这样的顶点，抛出异常。

参数  
label_id – Identifier for the label.  标签的标识符  
field_id – Identifier for the field.  字段的标识符  
field_value – The field value.  字段值  

返回  
The vertex by unique index.  通过唯一索引返回顶点  

```cpp
size_t GetNumVertices()
```
Gets the number of vertices.  
获取顶点的数量。

返回  
The number of vertices.  顶点的数量  

```cpp
const std::string &GetVertexPrimaryField(const std::string &label)
```
Gets vertex primary field  
获取顶点的主字段  

返回  
The primary field.  主字段  

```cpp
std::pair<uint64_t, uint64_t> Count()
```
Get the total number of vertex and edge  
获取顶点和边的总数  

返回  
std::pair object, first element is vertex number, second is edge number.  
返回一个 std::pair 对象，第一个元素是顶点数量，第二个元素是边的数量。  

```cpp
std::vector<std::tuple<bool, std::string, int64_t>> CountDetail()
```
Get the total number of vertex or edge for each label  
获取每个标签的顶点或边的总数  

返回  
std::tuple object list, first element indicates whether it is VERTEX or EDGE, second is label name, third is number.  
返回一个 std::tuple 对象列表，第一个元素指示它是 VERTEX 还是 EDGE，第二个是标签名称，第三个是数量。  

### Private Functions

#### explicit Transaction(lgraph::Transaction&&)  
显式事务

### Private Members

#### std::shared_ptr<lgraph::Transaction> txn_  
共享指针类型的事务  

### Friends

#### friend class GraphDB  
友元类 GraphDB  




















## lgraph_types

namespace lgraph_api

### Typedefs

typedef std::vector<std::pair<std::string, std::string>> EdgeConstraints  
// Edge constraints type define  
// 边约束类型定义

### Enums

enum class AccessLevel  
// Access level a user or role has on a graph. NONE: no permission. READ: read-only, no write access. WRITE: can read and write vertex and edge, but cannot change meta data such as schema or access. FULL: full access, can modify schema, grant access to other users, or even delete this graph.
// 用户或角色在图上的访问级别。NONE：没有权限。READ：只读，没有写入权限。WRITE：可以读取和写入顶点和边，但不能更改元数据，如模式或访问。FULL：完全访问，可以修改模式，授予其他用户访问权限，甚至删除此图。

Values:

- enumerator NONE  
- enumerator READ  
- enumerator WRITE  
- enumerator FULL  

enum class FieldAccessLevel  
// Field access level.  
// 字段访问级别。

Values:

- enumerator NONE  
- enumerator READ  
- enumerator WRITE  

enum class GraphQueryType  
// Graph query type.  
// 图查询类型。

Values:

- enumerator CYPHER  
- enumerator GQL  

enum FieldType  
// Field and value types.  
// 字段和值类型。

Values:

- enumerator NUL  
- enumerator NUL  
- enumerator NUL  
- enumerator BOOL  
- enumerator INT8  
- enumerator INT16  
- enumerator INT32  
- enumerator INT64  
- enumerator FLOAT  
- enumerator DOUBLE  
- enumerator DATE  
- enumerator DATETIME  
- enumerator STRING  
- enumerator BLOB  
- enumerator POINT  
- enumerator POINT  
- enumerator LINESTRING  
- enumerator LINESTRING  
- enumerator POLYGON  
- enumerator POLYGON  
- enumerator SPATIAL  
- enumerator FLOAT_VECTOR  

enum class LGraphType : uint16_t  
// a type of value used in result entry and parameter in procedure or plugin signature  
// 在结果条目和过程或插件签名中使用的值类型。

- Param INTEGER  
- Param FLOAT  
- Param DOUBLE  
- Param BOOLEAN  
- Param STRING  
- Param MAP  
  <string, FieldData>  
- Param NODE  
  VertexIterator, VertexId  
- Param RELATIONSHIP  
  InEdgeIterator || OutEdgeIterator, EdgeUid  
- Param PATH  
  lgraph_api::Path  
- Param LIST  
  <string, FieldData>  
- Param ANY  
  like Object in Java, its procedure author’s responsibility to check the underlying concrete type whether valid in runtime.  
  类似于Java中的对象，程序作者负责在运行时检查底层具体类型是否有效。

Values:

- enumerator NUL  
- enumerator INTEGER  
- enumerator FLOAT  
- enumerator DOUBLE  
- enumerator BOOLEAN  
- enumerator STRING  
- enumerator NODE  
- enumerator RELATIONSHIP  
- enumerator PATH  
- enumerator LIST  
- enumerator MAP  
- enumerator ANY  

enum PluginCodeType  
// Type of code given when loading a new plugin.  
// 加载新插件时给定的代码类型。

Values:

- enumerator PY  
- enumerator SO  
- enumerator CPP  
- enumerator ZIP  

enum class IndexType  
// index type  
// 索引类型。

Values:

- enumerator NonuniqueIndex  
  this is not unique index  
  // 这不是唯一索引
- enumerator GlobalUniqueIndex  
  this is a global unique index  
  // 这是一个全局唯一索引
- enumerator PairUniqueIndex  
  this is a pair unique index, for edge index only key of pair unique index is one of the follow case : if src_vid < dst_vid ,key is (index field value + src_vid + dst_vid) if src_vid > dst_vid ,key is (index field value + dst_vid + src_vid)  
  // 这是一个对唯一索引，仅边索引的对唯一索引的键是以下情况之一：如果src_vid < dst_vid，则键为（索引字段值 + src_vid + dst_vid），如果src_vid > dst_vid，则键为（索引字段值 + dst_vid + src_vid）

enum class CompositeIndexType  

Values:

- enumerator UniqueIndex  
  this is unique composite index  
  // 这是唯一复合索引
- enumerator NonUniqueIndex  
  this is not unique composite index  
  // 这不是唯一复合索引

### Functions

static inline std::string to_string(const AccessLevel &v)  
// Convert AccessLevel to string.  
// 将AccessLevel转换为字符串
static inline std::string to_string(const FieldAccessLevel &v)  
// Convert FieldAccessLevel to string.  
// 将FieldAccessLevel转换为字符串
static inline std::string to_string(const GraphQueryType &v)  
// Convert GraphQueryType to string.  
// 将GraphQueryType转换为字符串
inline const std::string to_string(FieldType v)  
// Get the name of the given FieldType.  
// 获取给定FieldType的名称。

抛出  
std::runtime_error – when an unrecognizable FieldType is given.  
// 当给定不可识别的FieldType时抛出。

参数  
v – A FieldType.  
// v – 一个FieldType。

返回  
Name of the given FieldType.  
// 给定FieldType的名称。

inline auto LGraphTypeIsField(LGraphType type) -> bool  
// Check if the type is a field type.  
// 检查该类型是否为字段类型
inline auto LGraphTypeIsGraphElement(LGraphType type) -> bool  
// Check if the type is a graph element type.  
// 检查该类型是否为图形元素类型
inline auto LGraphTypeIsCollection(LGraphType type) -> bool  
// Check if the type is a collection type.  
// 检查该类型是否为集合类型
inline auto LGraphTypeIsAny(LGraphType type) -> bool  
// Check if the type is ANY type.  
// 检查该类型是否为ANY类型
inline const std::string to_string(LGraphType type)  
// Convert LGraphType to string.  
// 将LGraphType转换为字符串
inline std::string PluginCodeTypeStr(PluginCodeType code_type)  
// Get the name of plugin code types.  
// 获取插件代码类型的名称。

struct CompositeIndexSpec  
#include <lgraph_types.h>  
// A composite index specifier.  
// 复合索引说明符。

Public Members

- std::string label  
  label name  
  // 标签名称
- std::vector<std::string> fields  
  fields name  
  // 字段名称
- CompositeIndexType type  

struct EdgeOptions : public LabelOptions  
#include <lgraph_types.h>  
// Edge label options, contain fields only edge have  
// 边标签选项，仅包含边具有的字段。

Public Types

enum class TemporalFieldOrder  
// Order of temporal field.  
// 时间字段的顺序。

Values:

- enumerator ASC  
- enumerator DESC  

Public Functions

EdgeOptions() = default  
// Default constructor.  
// 默认构造函数
inline explicit EdgeOptions(const EdgeConstraints &edge_constraints)  
// Construct EdgeOptions with edge constraints.  
// 根据边约束构造EdgeOptions
inline virtual std::string to_string() const  
// Convert EdgeOptions to string.  
// 将EdgeOptions转换为字符串
inline virtual void clear()  
// Clear EdgeOptions.  
// 清除EdgeOptions

Public Members

- EdgeConstraints edge_constraints  
- std::string temporal_field  
- enum lgraph_api::EdgeOptions::TemporalFieldOrder temporal_field_order = TemporalFieldOrder::ASC  

Public Static Functions

static inline std::string to_string(const TemporalFieldOrder &v)  
// Convert TemporalFieldOrder to string.  
// 将TemporalFieldOrder转换为字符串

### struct EdgeUid

#include <lgraph_types.h>

#### Public Functions

inline EdgeUid()  
// Default constructor for EdgeUid.  
// EdgeUid的默认构造函数
inline EdgeUid(int64_t s, int64_t d, uint16_t l, int64_t t, int64_t e)  
// Constructor for EdgeUid with parameters.  
// 带参数的EdgeUid构造函数
inline void Reverse()  
// Reverses side of this edge  
// 反转此边的方向

inline bool operator==(const EdgeUid &rhs) const  
// Equality operator for EdgeUid.  
// EdgeUid的相等运算符
inline bool operator!=(const EdgeUid &rhs) const  
// Inequality operator for EdgeUid.  
// EdgeUid的不等运算符
inline bool operator<(const EdgeUid &rhs) const  
// Less than operator for EdgeUid.  
// EdgeUid的小于运算符
inline bool operator>(const EdgeUid &rhs) const  
// Greater than operator for EdgeUid.  
// EdgeUid的大于运算符
inline std::string ToString() const  
// Get string representation of this object  
// 获取该对象的字符串表示

#### Public Members

- int64_t src  
  source vertex id  
  // 源顶点ID
- int64_t dst  
  destination vertex id  
  // 目的顶点ID
- uint16_t lid  
  label id  
  // 标签ID
- int64_t tid  
  timestamp  
  // 时间戳
- int64_t eid  
  additional edge id to distinguish edges with the same tid  
  // 额外的边ID以区分具有相同时间戳的边

#### Public Static Functions

static inline EdgeUid AnyEdge()  
// Get a generic edge identifier.  
// 获取通用边标识符

---

### struct Hash

#include <lgraph_types.h>

#### Public Functions

inline size_t operator()(const EdgeUid &edgeUid) const  
// Hash function for EdgeUid.  
// EdgeUid的哈希函数

---

### struct InEdgeSortOrder

#include <lgraph_types.h>  
// Comparator for EdgeUid of in-coming edges.  
// 传入边的EdgeUid的比较器。

#### Public Functions

inline bool operator()(const EdgeUid &lhs, const EdgeUid &rhs) const  
// Compare two EdgeUid for ordering.  
// 比较两个EdgeUid以进行排序

---

### struct OutEdgeSortOrder

#include <lgraph_types.h>  
// Comparator for EdgeUid of out-going edges.  
// 传出边的EdgeUid的比较器。

#### Public Functions

inline bool operator()(const EdgeUid &lhs, const EdgeUid &rhs) const  
// Compare two EdgeUid for ordering.  
// 比较两个EdgeUid以进行排序

---

### struct FieldData

#include <lgraph_types.h>  
// A class that represents variant type.  
// 表示变体类型的类。

#### Public Functions

inline FieldData()  
// Default constructor.  
// 默认构造函数
inline explicit FieldData(bool b)  
// Construct FieldData from bool.  
// 从布尔值构造FieldData
inline explicit FieldData(int8_t integer)  
// Construct FieldData from int8_t.  
// 从int8_t构造FieldData
inline explicit FieldData(int16_t integer)  
// Construct FieldData from int16_t.  
// 从int16_t构造FieldData
inline explicit FieldData(int32_t integer)  
// Construct FieldData from int32_t.  
// 从int32_t构造FieldData
inline explicit FieldData(int64_t integer)  
// Construct FieldData from int64_t.  
// 从int64_t构造FieldData
inline explicit FieldData(float real)  
// Construct FieldData from float.  
// 从浮点数构造FieldData
inline explicit FieldData(double real)  
// Construct FieldData from double.  
// 从双精度浮点数构造FieldData
inline explicit FieldData(const Date &d)  
// Construct FieldData from Date.  
// 从日期构造FieldData
inline explicit FieldData(const DateTime &d)  
// Construct FieldData from DateTime.  
// 从日期时间构造FieldData
inline explicit FieldData(const std::string &buf)  
// Construct FieldData from std::string.  
// 从std::string构造FieldData
inline explicit FieldData(std::string &&str)  
// Construct FieldData from std::string (move).  
// 从std::string（移动）构造FieldData
inline explicit FieldData(const char *buf)  
// Construct FieldData from C-string.  
// 从C字符串构造FieldData
inline explicit FieldData(const char *buf, size_t s)  
// Construct FieldData from C-string with size.  
// 从具有大小的C字符串构造FieldData
inline explicit FieldData(const Point<Cartesian> &p)  
// Construct FieldData from Cartesian Point.  
// 从笛卡尔点构造FieldData
inline explicit FieldData(const Point<Wgs84> &p)  
// Construct FieldData from Wgs84 Point.  
// 从Wgs84点构造FieldData
inline explicit FieldData(const LineString<Cartesian> &l)  
// Construct FieldData from Cartesian LineString.  
// 从笛卡尔线段构造FieldData
inline explicit FieldData(const LineString<Wgs84> &l)  
// Construct FieldData from Wgs84 LineString.  
// 从Wgs84线段构造FieldData
inline explicit FieldData(const Polygon<Cartesian> &p)  
// Construct FieldData from Cartesian Polygon.  
// 从笛卡尔多边形构造FieldData
inline explicit FieldData(const Polygon<Wgs84> &p)  
// Construct FieldData from Wgs84 Polygon.  
// 从Wgs84多边形构造FieldData
inline explicit FieldData(const Spatial<Cartesian> &s)  
// Construct FieldData from Cartesian Spatial.  
// 从笛卡尔空间构造FieldData
inline explicit FieldData(const Spatial<Wgs84> &s)  
// Construct FieldData from Wgs84 Spatial.  
// 从Wgs84空间构造FieldData
inline explicit FieldData(const std::vector<float> &fv)  
// Construct FieldData from a vector of floats.  
// 从浮点数向量构造FieldData
inline explicit FieldData(std::vector<float> &&fv)  
// Construct FieldData from a vector of floats (move).  
// 从浮点数向量（移动）构造FieldData
inline ~FieldData()  
// Destructor.  
// 析构函数
inline FieldData(const FieldData &rhs)  
// Copy constructor.  
// 拷贝构造函数
inline FieldData(FieldData &&rhs)  
// Move constructor.  
// 移动构造函数
inline FieldData &operator=(const FieldData &rhs)  
// Copy assignment operator.  
// 拷贝赋值运算符
inline FieldData &operator=(FieldData &&rhs)  
// Move assignment operator.  
// 移动赋值运算符

inline int64_t integer() const  
// Access the FieldData as int64. Valid only when the FieldData is of INT8, INT16, INT32, or INT64 types.  
// 将FieldData作为int64访问。仅当FieldData为INT8、INT16、INT32或INT64类型时有效。

抛出  
std::bad_cast – Thrown when the FieldData is not of int types.  
// 当FieldData不是整数类型时抛出。

返回  
An int64_t.  
// 一个int64_t。

inline double real() const  
// Access the FieldData as a double. The FieldData must be of FLOAT or DOUBLE types.
// 将FieldData作为双精度浮点数访问。FieldData必须是FLOAT或DOUBLE类型。

// 抛出  
std::bad_cast – Thrown if the FieldData is not of FLOAT or DOUBLE types.
// 抛出std::bad_cast – 如果FieldData不是FLOAT或DOUBLE类型，则抛出此异常。

// 返回  
A double.
// 返回一个双精度浮点数。

inline const std::string &string() const  
// Access the FieldData as std::string. Valid only for STRING, BLOB and SPATIAL. BLOB data is returned as-is, since std::string can also hold byte array.
// 将FieldData作为std::string访问。仅适用于STRING、BLOB和SPATIAL。BLOB数据原样返回，因为std::string也可以保存字节数组。

// 抛出  
std::bad_cast – Thrown when a bad cast error condition occurs.
// 抛出std::bad_cast – 当发生错误转换时抛出此异常。

// 返回  
A reference to a const std::string.
// 返回对const std::string的引用。

inline bool AsBool() const  
// Convert FieldData to bool.
// 将FieldData转换为布尔值。

inline int8_t AsInt8() const  
// Convert FieldData to INT8.
// 将FieldData转换为INT8。

inline int16_t AsInt16() const  
// Convert FieldData to INT16.
// 将FieldData转换为INT16。

inline int32_t AsInt32() const  
// Convert FieldData to INT32.
// 将FieldData转换为INT32。

inline int64_t AsInt64() const  
// Convert FieldData to INT64.
// 将FieldData转换为INT64。

inline float AsFloat() const  
// Convert FieldData to float.
// 将FieldData转换为浮点数。

inline double AsDouble() const  
// Convert FieldData to double.
// 将FieldData转换为双精度浮点数。

inline inline ::lgraph_api::Date AsDate () const  
// Convert FieldData to Date.
// 将FieldData转换为日期。

inline inline ::lgraph_api::DateTime AsDateTime () const  
// Convert FieldData to DateTime.
// 将FieldData转换为日期时间。

inline std::string AsString() const  
// Convert FieldData to std::string.
// 将FieldData转换为std::string。

inline std::string AsBlob() const  
// Convert FieldData to BLOB.
// 将FieldData转换为BLOB。

inline std::string AsBase64Blob() const  
// Convert FieldData to Base64 BLOB.
// 将FieldData转换为Base64 BLOB。

inline inline ::lgraph_api::SRID GetSRID () const  
// Get the SRID from the FieldData.
// 从FieldData中获取SRID。

inline inline ::lgraph_api::Point<::lgraph_api::Wgs84 > AsWgsPoint () const  
// Convert FieldData to WGS84 Point.
// 将FieldData转换为WGS84坐标点。

inline inline ::lgraph_api::Point<::lgraph_api::Cartesian > AsCartesianPoint () const  
// Convert FieldData to Cartesian Point.
// 将FieldData转换为笛卡尔坐标点。

inline inline ::lgraph_api::LineString<::lgraph_api::Wgs84 > AsWgsLineString () const  
// Convert FieldData to WGS84 LineString.
// 将FieldData转换为WGS84线串。

inline inline ::lgraph_api::LineString<::lgraph_api::Cartesian > AsCartesianLineString () const  
// Convert FieldData to Cartesian LineString.
// 将FieldData转换为笛卡尔线串。

inline inline ::lgraph_api::Polygon<::lgraph_api::Wgs84 > AsWgsPolygon () const  
// Convert FieldData to WGS84 Polygon.
// 将FieldData转换为WGS84多边形。

inline inline ::lgraph_api::Polygon<::lgraph_api::Cartesian > AsCartesianPolygon () const  
// Convert FieldData to Cartesian Polygon.
// 将FieldData转换为笛卡尔多边形。

inline inline ::lgraph_api::Spatial<::lgraph_api::Wgs84 > AsWgsSpatial () const  
// Convert FieldData to WGS84 Spatial data.
// 将FieldData转换为WGS84空间数据。

inline inline ::lgraph_api::Spatial<::lgraph_api::Cartesian > AsCartesianSpatial () const  
// Convert FieldData to Cartesian Spatial data.
// 将FieldData转换为笛卡尔空间数据。

inline std::vector<float> AsFloatVector() const  
// Convert FieldData to a vector of floats.
// 将FieldData转换为浮点数向量。

std::any ToBolt() const  
// Convert FieldData to a Bolt representation.
// 将FieldData转换为Bolt表示。

inline std::string ToString(const std::string &null_value = "NUL") const  
// Get string representation of this FieldData.
// 获取此FieldData的字符串表示。

inline bool operator==(const FieldData &rhs) const  
// Check for equality between two FieldData objects.
// 检查两个FieldData对象是否相等。

inline bool operator!=(const FieldData &rhs) const  
// Check for inequality between two FieldData objects.
// 检查两个FieldData对象是否不相等。

inline bool operator>(const FieldData &rhs) const  
// Compare if this FieldData is greater than another FieldData.
// 比较此FieldData是否大于另一个FieldData。

inline bool operator>=(const FieldData &rhs) const  
// Compare if this FieldData is greater than or equal to another FieldData.
// 比较此FieldData是否大于或等于另一个FieldData。

inline bool operator<(const FieldData &rhs) const  
// Compare if this FieldData is less than another FieldData.
// 比较此FieldData是否小于另一个FieldData。

inline bool operator<=(const FieldData &rhs) const  
// Compare if this FieldData is less than or equal to another FieldData.
// 比较此FieldData是否小于或等于另一个FieldData。

inline FieldType GetType() const  
// Get the type of the FieldData.
// 获取FieldData的类型。

inline bool is_null() const  
// Check if this FieldData is null.
// 检查此FieldData是否为空。

inline bool is_buf() const  
// Check if this FieldData is a buffer type.
// 检查此FieldData是否为缓冲区类型。

inline bool is_empty_buf() const  
// Check if this FieldData is an empty buffer.
// 检查此FieldData是否为空缓冲区。

inline bool IsNull() const  
// Query if this object is null.
// 查询此对象是否为空。

inline bool IsBool() const  
// Query if this object is bool.
// 查询此对象是否为布尔值。

inline bool IsBlob() const  
// Query if this object is BLOB.
// 查询此对象是否为BLOB。

inline bool IsString() const  
// Query if this object is string.
// 查询此对象是否为字符串。

inline bool IsInt8() const  
// Query if this object is INT8.
// 查询此对象是否为INT8。

inline bool IsInt16() const  
// Query if this object is INT16.
// 查询此对象是否为INT16。

inline bool IsInt32() const  
// Query if this object is INT32.
// 查询此对象是否为INT32。

inline bool IsInt64() const  
// Query if this object is INT64.
// 查询此对象是否为INT64。

inline bool IsInteger() const  
// Is this a INT8, INT16, INT32 or INT64?
// 这是否为INT8、INT16、INT32或INT64？

inline bool IsFloat() const  
// Query if this object is float.
// 查询此对象是否为浮点数。

inline bool IsDouble() const  
// Query if this object is double.
// 查询此对象是否为双精度浮点数。

inline bool IsReal() const  
// Is this a FLOAT or DOUBLE?
// 这是否为FLOAT或DOUBLE？

inline bool IsDate() const  
// Query if this object is date.
// 查询此对象是否为日期。

inline bool IsDateTime() const  
// Query if this object is date time.
// 查询此对象是否为日期时间。

inline bool IsPoint() const  
// Query if this object is Point.
// 查询此对象是否为点。

inline bool IsLineString() const  
// Query if this object is LineString.
// 查询此对象是否为线串。

inline bool IsPolygon() const  
// Query if this object is Polygon.
// 查询此对象是否为多边形。

inline bool IsSpatial() const  
// Query if this object is spatial.
// 查询此对象是否为空间数据。

inline bool IsFloatVector() const  
// Query if this object is float vector.
// 查询此对象是否为浮点数向量。

#### Public Members

- FieldType type  
// The type of the FieldData.  
// FieldData的类型。

- bool boolean  
// Boolean value of the FieldData.  
// FieldData的布尔值。

- int8_t int8  
// INT8 value of the FieldData.  
// FieldData的INT8值。

- int16_t int16  
// INT16 value of the FieldData.  
// FieldData的INT16值。

- int32_t int32  
// INT32 value of the FieldData.  
// FieldData的INT32值。

- int64_t int64  
// INT64 value of the FieldData.  
// FieldData的INT64值。

- float sp  
// FLOAT value of the FieldData.  
// FieldData的FLOAT值。

- double dp  
// DOUBLE value of the FieldData.  
// FieldData的DOUBLE值。

- std::string *buf  
// Buffer of the FieldData.  
// FieldData的缓冲区。

- std::vector<float> *vp  
// Vector of floats in the FieldData.  
// FieldData中的浮点数向量。

- union lgraph_api::FieldData::[anonymous] data  
// Anonymous union to hold different types of data.
// 匿名联合用于存放不同类型的数据。

#### Public Static Functions

static inline FieldData Bool(bool b)  // 创建一个布尔值的字段数据
static inline FieldData Int8(int8_t i)  // 创建一个8位整数的字段数据
static inline FieldData Int16(int16_t i)  // 创建一个16位整数的字段数据
static inline FieldData Int32(int32_t i)  // 创建一个32位整数的字段数据
static inline FieldData Int64(int64_t i)  // 创建一个64位整数的字段数据
static inline FieldData Float(float d)  // 创建一个浮点数的字段数据
static inline FieldData Double(double d)  // 创建一个双精度浮点数的字段数据
static inline FieldData Date(const std::string &str)  // 创建一个日期的字段数据，从字符串转换
static inline FieldData Date(const ::lgraph_api::Date &d)  // 创建一个日期的字段数据，从日期对象转换
static inline FieldData DateTime(const std::string &str)  // 创建一个日期时间的字段数据，从字符串转换
static inline FieldData DateTime(const ::lgraph_api::DateTime &d)  // 创建一个日期时间的字段数据，从日期时间对象转换
static inline FieldData String(const std::string &str)  // 创建一个字符串的字段数据
static inline FieldData String(std::string &&str)  // 创建一个字符串的字段数据，从右值引用
static inline FieldData String(const char *str)  // 创建一个字符串的字段数据，从C风格字符串
static inline FieldData String(const char *p, size_t s)  // 创建一个字符串的字段数据，从C风格字符串及其大小
static inline FieldData Point(const ::lgraph_api::Point<Cartesian> &p)  // 创建一个笛卡尔坐标点的字段数据
static inline FieldData Point(const ::lgraph_api::Point<Wgs84> &p)  // 创建一个WGS84坐标点的字段数据
static inline FieldData Point(const std::string &str)  // 创建一个点的字段数据，从字符串转换
static inline FieldData LineString(const ::lgraph_api::LineString<Cartesian> &l)  // 创建一个笛卡尔坐标线串的字段数据
static inline FieldData LineString(const ::lgraph_api::LineString<Wgs84> &l)  // 创建一个WGS84坐标线串的字段数据
static inline FieldData LineString(const std::string &str)  // 创建一个线串的字段数据，从字符串转换
static inline FieldData Polygon(const ::lgraph_api::Polygon<Cartesian> &p)  // 创建一个笛卡尔坐标多边形的字段数据
static inline FieldData Polygon(const ::lgraph_api::Polygon<Wgs84> &p)  // 创建一个WGS84坐标多边形的字段数据
static inline FieldData Polygon(const std::string &str)  // 创建一个多边形的字段数据，从字符串转换
static inline FieldData Spatial(const ::lgraph_api::Spatial<Cartesian> &s)  // 创建一个笛卡尔坐标空间数据的字段数据
static inline FieldData Spatial(const ::lgraph_api::Spatial<Wgs84> &s)  // 创建一个WGS84坐标空间数据的字段数据
static inline FieldData Spatial(const std::string &str)  // 创建一个空间数据的字段数据，从字符串转换
static inline FieldData FloatVector(const std::vector<float> &fv)  // 创建一个浮点向量的字段数据
static inline FieldData Blob(const std::string &str)  // 从字符串构建BLOB字段数据
static inline FieldData Blob(std::string &&str)  // 从右值引用字符串构建BLOB字段数据
static inline FieldData Blob(const std::vector<uint8_t> &str)  // 从uint8_t的向量构建BLOB字段数据, 将其视为字节数组。
Constructs a Blob from vector of uint8_t, treated as byte array.

static inline FieldData BlobFromBase64(const std::string &base64_encoded)  // 从Base64编码的字符串构建BLOB字段数据
Constructs a BLOB from Base64 encoded string.

#### Private Static Functions

static inline bool IsBufType(FieldType t)  // 查询“t”是否为BLOB或STRING类型
Query if ‘t’ is BLOB or STRING

static inline bool IsInteger(FieldType t)  // 查询“t”是否为INT8, 16, 32或者64类型
Query if ‘t’ is INT8, 16, 32, or 64

static inline bool IsReal(FieldType t)  // 查询“t”是否为FLOAT或DOUBLE类型
Query if ‘t’ is FLLOAT or DOUBLE

### struct FieldSpec

#include <lgraph_types.h>  // 引入lgraph_types.h文件
Specification for a field.

#### Public Functions

inline FieldSpec()  // 默认构造函数
inline FieldSpec(const std::string &n, FieldType t, bool nu)  // 构造函数
Constructor  

**参数**  
n – Field name  // 字段名称  

t – Field type  // 字段类型  

nu – True if field is optional  // 如果字段是可选的则为真  

inline FieldSpec(std::string &&n, FieldType t, bool nu)  // 从右值引用构造函数
inline bool operator==(const FieldSpec &rhs) const  // 重载相等运算符
inline std::string ToString() const  // 获取FieldSpec的字符串表示
Get the string representation of the FieldSpec.

#### Public Members

- std::string name  // 字段的名称
  name of the field  
- FieldType type  // 字段的类型
  type of that field  
- bool optional  // 该字段是否可选？
  is this field optional?  

---

### struct IndexSpec

#include <lgraph_types.h>  
An index specifier. // 索引说明符

#### Public Members

- std::string label  
  label name  // 标签名称  
- std::string field  
  field name  // 字段名称  
- IndexType type  // 索引类型  

---

### struct LabelOptions

#include <lgraph_types.h>  
Label options, base class, define some common fields and methods // 标签选项，基类，定义一些通用字段和方法

**Subclassed by** EdgeOptions, VertexOptions // 由 EdgeOptions 和 VertexOptions 继承

#### Public Functions

virtual std::string to_string() const = 0  
// 返回对象的字符串表示形式  
virtual void clear() = 0  
// 清除对象的状态  
inline virtual ~LabelOptions()  
// 析构函数  

#### Public Members

- bool detach_property = false  
// 是否分离属性  

---

### struct Parameter

#include <lgraph_types.h>  
The parameter of procedure/plugin // 过程/插件的参数

#### Public Members

- std::string name  
  name of the parameter  // 参数名称  
- int index  
  index of the parameter list in which the parameter stay  // 参数在参数列表中的索引  

---

### struct RoleInfo

#include <lgraph_types.h>  
Information about the role. // 角色的信息

#### Public Members

- std::string desc  
  description  // 描述  
- std::map<std::string, AccessLevel> graph_access  
  access levels on different graphs  // 在不同图上的访问级别  
- bool disabled = false  
  is this role disabled?  // 此角色是否被禁用?  

---

### struct SigSpec

#include <lgraph_types.h>  

#### Public Members

- std::vector<Parameter> input_list  
// 输入参数列表  
- std::vector<Parameter> result_list  
  input parameter list  // 输入参数列表  

---

### struct UserInfo

#include <lgraph_types.h>  
Information about the user. // 用户的信息

#### Public Members

- std::string desc  
  description of the user  // 用户的描述  
- std::set<std::string> roles  
  roles of this user  // 此用户的角色  
- bool disabled = false  
  is this user disabled?  // 此用户是否被禁用?  
- size_t memory_limit  
  memory limit for this user  // 此用户的内存限制  

---

### struct VectorIndexSpec

#include <lgraph_types.h>  

#### Public Members

- std::string label  
// 标签  
- std::string field  
// 字段  
- std::string index_type  
// 索引类型  
- int dimension  
// 维度  
- std::string distance_type  
// 距离类型  
- int hnsm_m  
// hnsm 的 m 值  
- int hnsm_ef_construction  
// hnsm 的 ef_construction 值  

---

### struct VertexOptions : public LabelOptions

#include <lgraph_types.h>  
Vertex label options, contain fields only vertex have // 顶点标签选项，仅包含顶点拥有的字段

#### Public Functions

VertexOptions() = default  
// 默认构造函数  
inline explicit VertexOptions(const std::string &primary_field)  
// 带有主字段的显式构造函数  
inline virtual std::string to_string() const  
// 返回对象的字符串表示形式  
inline virtual void clear()  
// 清除对象的状态  

#### Public Members

- std::string primary_field  
// 主字段  



















## lgraph_utils

### Typedefs
```cpp
using json = nlohmann::json // 使用 nlohmann::json 作为 json 的类型别名
```

### Namespace lgraph_api

### Functions
```cpp
double get_time() 
```
Get current time.  // 获取当前时间。

返回  
Digit value of current time.  // 当前时间的数字值。

```cpp
void split_string(std::string origin_string, std::vector<std::string> &sub_strings, std::string string_delimiter) 
```
Split the original string by format.  //按照某种格式分割原始字符串。

参数  
origin_string – Original string to be split.  // 原始字符串。  
sub_strings – Split substring.  // 分割后的子字符串。  
string_delimiter – Split format.  // 分割格式。

```cpp
std::string rc4(std::string &input, std::string key, std::string mode) 
```
Encrypt the input string in RC4 format.  // 使用 RC4 格式加密输入字符串。

参数  
input – Input string.  // 输入字符串。  
key – Encryption key.  // 加密密钥。  
mode – Encryption mode.  // 加密模式。

返回  
Encrypted string.  // 加密后的字符串。

```cpp
std::string encode_base64(const std::string input) 
```
Encode the input string in Base64 format.  // 将输入字符串编码为 Base64 格式。

参数  
input – Input string.  // 输入字符串。

返回  
Encrypted string.  // 编码后的字符串。

```cpp
std::string decode_base64(const std::string input) 
```
Decode the input string in Base64 format.  // 将输入字符串解码为 Base64 格式。

参数  
input – Input string.  // 输入字符串。

返回  
Decrypted string.  // 解码后的字符串。

```cpp
void *alloc_buffer(size_t bytes) 
```
Allocate memory with size in bytes.  // 按字节数分配内存。

参数  
bytes – Size in bytes of requested memory.  // 请求内存的字节大小。

返回  
Pointer of allocated memory.  // 分配内存的指针。

```cpp
void dealloc_buffer(void *buffer, size_t bytes) 
```
Free memory with size in bytes.  // 按字节数释放内存。

参数  
buffer – Pointer of memory to free.  // 要释放的内存指针。  
bytes – Size in bytes of requested memory.  // 请求内存的字节大小。

```cpp
template<class DataType>
void parse_from_json(DataType &value, const char *key, json &input) 
```
Parse parameter from `nlohmann::json`.  // 从 `nlohmann::json` 中解析参数。

参数  
value – [out] Value to store parameter.  // [输出] 存储参数的值。  
key – Key of the parameter in the input.  // 输入中参数的键。  
input – Input JSON.  // 输入的 JSON。

```cpp
template<class DataType>
void parse_from_json(std::vector<DataType> &value, const char *key, json &input) 
```
Parse vector parameter from `nlohmann::json`.  // 从 `nlohmann::json` 中解析向量参数。

参数  
value – [out] Value to store parameter.  // [输出] 存储参数的值。  
key – Key of the parameter in the input.  // 输入中参数的键。  
input – Input JSON.  // 输入的 JSON。

```cpp
size_t GetVidFromNodeString(const std::string &node_string) 
```
Parse vid from the node passed in by Cypher. For V2 procedure.  // 从 Cypher 传入的节点中解析 vid。适用于 V2 过程。

参数  
node_string – [in] Node.  // 节点。

返回  
vid.  // vid。
















## lgraph_vertex_index_iterator

### Namespace
- `lgraph`
- `lgraph_api`

### Class VertexIndexIterator
```cpp
#include <lgraph_vertex_index_iterator.h>
```
`VertexIndexIterator` can be used to access a set of vertices that has the same indexed value. If the index is unique (that is, each vertex has a unique index value), then each `VertexIndexIterator` will only have one `VertexId`, and will become invalid after `Next()` is called.

`VertexIndexIterator` 可以用于访问具有相同索引值的一组顶点。如果索引是唯一的（即每个顶点有一个唯一的索引值），那么每个 `VertexIndexIterator` 只会有一个 `VertexId`，并且在调用 `Next()` 后会变得无效。

A `VertexIndexIterator` is valid iff it points to a valid `(index_value, vid)` pair; otherwise, it is invalid. Calling member functions on an invalid `VertexIndexIterator` throws an exception, except for the `IsValid()` function.

`VertexIndexIterator` 是有效的当且仅当它指向一个有效的 `(index_value, vid)` 对；否则，它是无效的。在无效的 `VertexIndexIterator` 上调用成员函数会抛出异常，除了 `IsValid()` 函数。

#### Public Functions
```cpp
VertexIndexIterator(VertexIndexIterator &&rhs)
```
构造函数，移动构造 `VertexIndexIterator`。

```cpp
VertexIndexIterator &operator=(VertexIndexIterator&&)
```
移动赋值运算符。

```cpp
~VertexIndexIterator()
```
析构函数。

```cpp
void Close()
```
Closes this iterator.

关闭此迭代器。

```cpp
bool IsValid() const
```
Query if this iterator is valid, i.e., the Key and Vid can be queried.

查询此迭代器是否有效，即 Key 和 Vid 是否可以被查询。

返回  
True if valid, false if not.

返回  
如果有效则返回 true，否则返回 false。

```cpp
bool Next()
```
Move to the next vertex id in the list, which consists of all the valid vertex ids of the iterator and is sorted from small to large. If we hit the end of the list, the iterator will become invalid and false is returned.

移动到列表中的下一个顶点 id，该列表由迭代器的所有有效顶点 id 组成，并按从小到大排序。如果到达列表末尾，则迭代器将变得无效，返回 false。

返回  
True if it succeeds, otherwise false.

返回  
如果成功则返回 true，否则返回 false。

```cpp
FieldData GetIndexValue() const
```
Gets the current index value. The vids are sorted in `(IndexValue, Vid)` order. When `Next()` is called, the iterator moves from one vid to next, possibly moving from one IndexValue to another. This function tells the IndexValue currently pointed to.

获取当前的索引值。vids 按 `(IndexValue, Vid)` 顺序排序。当调用 `Next()` 时，迭代器从一个 vid 移动到下一个，可能会从一个 IndexValue 移动到另一个。此函数返回当前指向的 IndexValue。

返回  
The key.

返回  
键（key）。

```cpp
int64_t GetVid() const
```
Gets the current vertex id.

获取当前顶点 id。

返回  
The current vertex id.

返回  
当前顶点 id。

#### Private Functions
```cpp
VertexIndexIterator(lgraph::VertexIndexIterator &&it, const std::shared_ptr<lgraph::Transaction> &txn)
```

```cpp
VertexIndexIterator(const VertexIndexIterator&) = delete
```
拷贝构造函数被删除，禁止使用拷贝构造。

```cpp
VertexIndexIterator &operator=(const VertexIndexIterator&) = delete
```
拷贝赋值运算符被删除，禁止使用拷贝赋值。

#### Private Members
- `std::unique_ptr<lgraph::VertexIndexIterator> it_`
- `std::shared_ptr<lgraph::Transaction> txn_`

#### Friends
- `friend class Transaction`















## lgraph_vertex_iterator

### Namespace
- `lgraph`
- `graph`
- `lgraph_api`

### Class VertexIterator
```cpp
#include <lgraph_vertex_iterator.h>
```
`VertexIterator` can be used to iterate through vertices in the DB. Vertices are sorted according to vertex id in the DB.  
`VertexIterator`可以用来遍历数据库中的顶点。顶点根据数据库中的顶点ID进行排序。

A `VertexIterator` is valid iff it points to a valid vertex. Calling method functions on an invalid `VertexIterator` throws an `InvalidIterator`, except for the `IsValid()` and `Goto()` functions.  
当`VertexIterator`指向一个有效顶点时，才被认为是有效的。对无效的`VertexIterator`调用方法函数将抛出`InvalidIterator`，但`IsValid()`和`Goto()`函数除外。

The following operations invalidate a `VertexIterator`:  
以下操作将使`VertexIterator`无效：
- Constructing a `VertexIterator` for a non-existing vertex.  
  为不存在的顶点构造`VertexIterator`。
- Calling `Goto()` with the id of a non-existing vertex.  
  使用不存在的顶点ID调用`Goto()`。
- Calling `Next()` on the last vertex.  
  在最后一个顶点上调用`Next()`。
- Calling `Delete()` on the last vertex.  
  在最后一个顶点上调用`Delete()`。

#### Public Functions
```cpp
VertexIterator(VertexIterator &&rhs)
```
```cpp
VertexIterator &operator=(VertexIterator &&rhs)
```
```cpp
~VertexIterator()
```
```cpp
void Close()
```
Closes this iterator.  
关闭此迭代器。

```cpp
bool Next()
```
Move to the next vertex.  
移动到下一个顶点。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
True if it succeeds, otherwise return false (no more vertex) and invalidate the iterator.  
如果成功则返回True，否则返回false（没有更多顶点）并使迭代器无效。

```cpp
bool Goto(int64_t vid, bool nearest = false)
```
Goto the vertex with id `src`. If there is no vertex with exactly the same vid, and `nearest==true`, go to the next vertex with id >= vid; otherwise, return false and invalidate the iterator.  
跳转到ID为`src`的顶点。如果没有与给定vid完全相同的顶点，并且`nearest==true`，则跳转到ID >= vid的下一个顶点；否则返回false并使迭代器无效。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出

参数  
`vid` – Vertex id of the vertex to go.  // 要跳转的顶点ID  
`nearest` – (Optional) True to go to the closest vertex with id >= vid.  // （可选）如果设置为True，跳转到ID >= vid的最近顶点

返回  
True if it succeeds, otherwise false (no such vertex).  
如果成功则返回True，否则返回false（没有这样的顶点）。

```cpp
int64_t GetId() const
```
Gets the vertex id.  
获取顶点ID。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
The id.  
返回ID。

```cpp
OutEdgeIterator GetOutEdgeIterator() const
```
Gets an `OutEdgeIterator` pointing to the first out-going edge.  
获取指向第一个出边的`OutEdgeIterator`。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
The `OutEdgeIterator`.  
返回`OutEdgeIterator`。

```cpp
OutEdgeIterator GetOutEdgeIterator(const EdgeUid &euid, bool nearest = false) const
```
Returns an `OutEdgeIterator` pointing to the edge specified by `euid`. If there is no such edge, and `nearest==false`, an invalid iterator is returned. If the specified out-edge does not exist, and `nearest==true`, get the first out-edge that sorts after the specified one.  
返回指向由`euid`指定的边的`OutEdgeIterator`。如果没有这样的边，并且`nearest==false`，则返回无效迭代器。如果指定的出边不存在并且`nearest==true`，则获取排序在指定边之后的第一个出边。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

参数  
`euid` – The Edge Unique Id.  // 边的唯一ID  
`nearest` – (Optional) If set to true and the specified edge does not exist, get the edge that sorts right after it.  // （可选）如果设置为true且指定边不存在，则获取排序在其后面的边。

返回  
The out edge iterator.  
出边迭代器。

```cpp
InEdgeIterator GetInEdgeIterator() const
```
Gets an `InEdgeIterator` pointing to the first in-coming edge.  
获取指向第一个入边的`InEdgeIterator`。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
The `InEdgeIterator`.  
返回`InEdgeIterator`。

```cpp
InEdgeIterator GetInEdgeIterator(const EdgeUid &euid, bool nearest = false) const
```
Returns an `InEdgeIterator` pointing to the edge specified by `euid`. If there is no such edge and `nearest==false`, an invalid iterator is returned. If the specified edge does not exist and `nearest==true`, get the edge that sorts right after it.  
返回指向由`euid`指定的边的`InEdgeIterator`。如果没有这样的边并且`nearest==false`，则返回无效迭代器。如果指定的边不存在并且`nearest==true`，则获取排序在其后面的边。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

参数  
`euid` – The Edge Unique Id.  // 边的唯一ID  
`nearest` – (Optional) If set to true and the specified edge does not exist, get the edge that sorts right after it.  // （可选）如果设置为true且指定边不存在，则获取排序在其后面的边。

返回  
The `InEdgeIterator`.  
返回`InEdgeIterator`。

```cpp
bool IsValid() const
```
Query if this iterator is valid.  
查询此迭代器是否有效。

返回  
True if valid, false if not.  
如果有效，则返回True；否则返回false。

```cpp
const std::string &GetLabel() const
```
Gets the label of this vertex.  
获取此顶点的标签。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
The label.  
标签。

```cpp
size_t GetLabelId() const
```
Gets label id of this vertex.  
获取此顶点的标签ID。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出

返回  
The label identifier.  
标签标识符。

```cpp
std::vector<FieldData> GetFields(const std::vector<std::string> &field_names) const
```
Gets the fields specified.  
获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出  
`InputError` – Thrown on other input errors (field not exist, etc.).  // 对于其他输入错误（字段不存在等）抛出

参数  
`field_names` – List of names of the fields.  // 字段名称列表

返回  
The fields.  
字段。

```cpp
FieldData GetField(const std::string &field_name) const
```
Gets the field specified.  
获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出  
`InputError` – Thrown on other input errors (field not exist, etc.).  // 对于其他输入错误（字段不存在等）抛出

参数  
`field_name` – Field name.  // 字段名称

返回  
Field value.  
字段值。

```cpp
std::vector<FieldData> GetFields(const std::vector<size_t> &field_ids) const
```
Gets the fields specified.  
获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出  
`InputError` – Thrown on other input errors (field not exist, etc.).  // 对于其他输入错误（字段不存在等）抛出

参数  
`field_ids` – List of ids for the fields.  // 字段ID列表

返回  
The fields.  
字段。

```cpp
FieldData GetField(size_t field_id) const
```
Gets the field specified.  
获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  // 当在无效事务中调用时抛出  
`InvalidIterator` – Thrown when current iterator is invalid.  // 当当前迭代器无效时抛出  
`InputError` – Thrown on other input errors (field not exist, etc.).  // 对于其他输入错误（字段不存在等）抛出

参数  
`field_id` – Field ID.  // 字段ID

返回  
Field value.  
字段值。

```cpp
inline FieldData operator[](const std::string &field_name) const
```
Get field identified by `field_name`.  
通过 `field_name` 获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_name` – Filename of the file.  
字段的名称。

返回  
The indexed value.  
索引值。

```cpp
inline FieldData operator[](size_t fid) const
```
Get field identified by field id.  
通过字段 ID 获取指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`fid` – The field id.  
字段 ID。

返回  
The indexed value.  
索引值。

```cpp
std::map<std::string, FieldData> GetAllFields() const
```
Gets all fields of the current vertex.  
获取当前顶点的所有字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。

返回  
All fields.  
所有字段。

```cpp
void SetField(const std::string &field_name, const FieldData &field_value)
```
Sets the specified field.  
设置指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_name` – Field name.  
字段名。  
`field_value` – Field value.  
字段值。

```cpp
void SetField(size_t field_id, const FieldData &field_value)
```
Sets the specified field.  
设置指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_id` – Field id.  
字段 ID。  
`field_value` – Field value.  
字段值。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<std::string> &field_value_strings)
```
Sets the fields specified.  
设置指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_names` – List of names of the fields.  
字段名称列表。  
`field_value_strings` – The field value strings.  
字段值字符串。

```cpp
void SetFields(const std::vector<std::string> &field_names, const std::vector<FieldData> &field_values)
```
Sets the fields specified.  
设置指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_names` – List of names of the fields.  
字段名称列表。  
`field_values` – The field values.  
字段值。

```cpp
void SetFields(const std::vector<size_t> &field_ids, const std::vector<FieldData> &field_values)
```
Sets the fields specified.  
设置指定的字段。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。  
`InputError` – Thrown on other input errors (field not exist, etc.).  
在其他输入错误（字段不存在等）时抛出。

参数  
`field_ids` – List of identifiers for the fields.  
字段的标识符列表。  
`field_values` – The field values.  
字段值。

```cpp
std::vector<int64_t> ListSrcVids(size_t n_limit = std::numeric_limits<size_t>::max(), bool *more_to_go = nullptr)
```
List source vids. Each source vid is stored only once in the result.  
列出源 vid。每个源 vid 在结果中只存储一次。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。

参数  
`n_limit` – (Optional) The limit on the number of vids to return.  
（可选）返回的 vid 数量限制。  
`more_to_go` – [out] (Optional) If non-null, returns whether the limit is exceeded.  
[输出]（可选）如果不为 null，返回是否超出限制。

返回  
List of source vids.  
源 vid 列表。

```cpp
std::vector<int64_t> ListDstVids(size_t n_limit = std::numeric_limits<size_t>::max(), bool *more_to_go = nullptr)
```
List destination vids. Each vid is stored only once in the result.  
列出目标 vid。每个 vid 在结果中只存储一次。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。

参数  
`n_limit` – (Optional) The limit of the number of vids to return.  
（可选）返回的 vid 数量限制。  
`more_to_go` – [out] (Optional) If non-null, returns whether the limit is exceeded.  
[输出]（可选）如果不为 null，返回是否超出限制。

返回  
List of destination vids.  
目标 vid 列表。

```cpp
size_t GetNumInEdges(size_t n_limit = std::numeric_limits<size_t>::max(), bool *more_to_go = nullptr)
```
Gets number of incoming edges, stopping on limit. This function can come in handy if we need to filter on large vertexes.  
获取传入边的数量，在限制时停止。如果我们需要对大型顶点进行过滤，这个函数可能会很有用。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。

参数  
`n_limit` – (Optional) The limit on the number of in-coming edges to count. When the limit is reached, `n_limit` is returned.  
（可选）要计算的传入边的数量限制。当达到限制时，返回 `n_limit`。  
`more_to_go` – [out] (Optional) If non-null, return whether the limit is exceeded.  
[输出]（可选）如果不为 null，返回是否超出限制。

返回  
Number of incoming edges.  
传入边的数量。

```cpp
size_t GetNumOutEdges(size_t n_limit = std::numeric_limits<size_t>::max(), bool *more_to_go = nullptr)
```
Gets number of out-going edges, stopping on limit. This function can come in handy if we need to filter on large vertexes.  
获取传出边的数量，在限制时停止。如果我们需要对大型顶点进行过滤，这个函数可能会很有用。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。

参数  
`n_limit` – (Optional) The limit on the number of out-going edges to count. When the limit is reached, `n_limit` is returned and `limit_exceeded` is set to true.  
（可选）要计算的传出边的数量限制。当达到限制时，返回 `n_limit`，并将 `limit_exceeded` 设置为 true。  
`more_to_go` – [out] (Optional) If non-null, return whether the limit is exceeded.  
[输出]（可选）如果不为 null，返回是否超出限制。

返回  
Number of out-going edges.  
传出边的数量。

```cpp
void Delete(size_t *n_in_edges = nullptr, size_t *n_out_edges = nullptr)
```
Deletes this vertex, also deletes all incoming and outgoing edges of this vertex. The iterator will point to the next vertex by vid if there is any.  
删除该顶点，同时删除该顶点的所有传入和传出边。如果有下一个顶点，迭代器将指向其 vid。

抛出  
`InvalidTxn` – Thrown when called inside an invalid transaction.  
当在无效事务中调用时抛出。  
`InvalidIterator` – Thrown when current iterator is invalid.  
当当前迭代器无效时抛出。  
`WriteNotAllowed` – Thrown when called in a read-only transaction.  
在只读事务中调用时抛出。

参数  
`n_in_edges` – [out] (Optional) If non-null, the number of in edges the vertex had.  
[输出]（可选）如果不为 null，该顶点的传入边数量。  
`n_out_edges` – [out] (Optional) If non-null, the number of out edges the vertex had.  
[输出]（可选）如果不为 null，该顶点的传出边数量。

```cpp
std::string ToString() const
```
Get the string representation of this vertex.  
获取该顶点的字符串表示。

#### Private Functions
```cpp
VertexIterator(lgraph::graph::VertexIterator&&, const std::shared_ptr<lgraph::Transaction>&)
```

```cpp
VertexIterator(const VertexIterator&) = delete
```

```cpp
VertexIterator &operator=(const VertexIterator&) = delete
```

#### Private Members
```cpp
std::unique_ptr<lgraph::graph::VertexIterator> it_
```

```cpp
std::shared_ptr<lgraph::Transaction> txn_
```

#### Friends
friend class Transaction

















## olap_base

这是TuGraph图分析引擎的实现。图分析引擎是一个通用的处理引擎，适用于实现各种图分析算法，例如PageRank、ShortestPath等。

### Defines
- **THREAD_WORKING**  
  - 线程工作模式。
  
- **THREAD_STEALING**  
  - 线程窃取模式。

- **VERTEX_BATCH_SIZE**  
  - 顶点批处理大小。

- **WORD_OFFSET(i)**  
  - 字的偏移量宏定义。

- **BIT_OFFSET(i)**  
  - 位的偏移量宏定义。

### Functions
- union @5 `__attribute__ ((packed))`  
  - 定义一个打包的联合体。

### Variables
- `size_t neighbour`  
  - 邻居的数量或标识符。

- `EdgeData edge_data`  
  - 边的数据。

- `size_t src`  
  - 边的源顶点标识符。

- `size_t dst`  
  - 边的目的顶点标识符。

### Namespaces
- `namespace lgraph_api`  
  - 图形API的命名空间。

- `namespace olap`  
  - OLAP的命名空间。

#### Enums
- **enum EdgeDirectionPolicy**  
  定义图的边方向策略。该策略确定图的对称性和无向特性。

  **Values:**
  - **enumerator DUAL_DIRECTION**  
    图是非对称的。输入文件中的边是出边。反向边形成入边。
  
  - **enumerator MAKE_SYMMETRIC**  
    图是对称的，但输入文件是非对称的。出边和入边是相同的。
  
  - **enumerator INPUT_SYMMETRIC**  
    图和输入文件都是对称的。出边和入边是相同的。

#### Functions
- **template<typename EdgeData> struct lgraph_api::olap::AdjUnit `__attribute__ ((packed))`**  
  - 定义相邻边的结构，使用EdgeData作为权重类型。

- **template<> struct lgraph_api::olap::AdjUnit< Empty > `__attribute__ ((packed))`**  
  - 对于无权图的特化AdjUnit结构定义。

- **template<typename ReducedSum> static ReducedSum reduce_plus(ReducedSum a, ReducedSum b)**  
  默认的归约函数，使用加法运算符。

- **template<typename T> T ForEachVertex(GraphDB &db, Transaction &txn, std::vector<Worker> &workers, const std::vector<int64_t> &vertices, std::function<void(Transaction&, VertexIterator&, T&)> work, std::function<void(const T&, T&)> reduce, size_t parallel_factor = 8)**  
  遍历每个顶点并执行指定的工作。

- **template<typename T> std::vector<T> ForEachVertex(GraphDB &db, Transaction &txn, std::vector<Worker> &workers, const std::vector<int64_t> &vertices, std::function<T(Transaction&, VertexIterator&, size_t)> work, size_t parallel_factor = 8)**  
  遍历每个顶点并生成结果向量。

#### Variables
- **struct lgraph_api::olap::EdgeStringUnit `__attribute__`**  
  - 表示一个带有字符串类型顶点的边的结构。

- **static constexpr size_t MAX_NUM_EDGES = 1ul << 36**  
  最大边数。如果需要，可以更改此值。

- **template<typename EdgeData> class AdjList**  
  ```cpp
  #include <olap_base.h>
  ```
  - 表示一个邻接列表的类。

##### Public Functions
- **inline AdjList()**  
  - 默认构造函数。

- **inline AdjUnit<EdgeData> *begin()**  
  - 获取邻接列表的开始迭代器。

- **inline AdjUnit<EdgeData> *end()**  
  - 获取邻接列表的结束迭代器。

- **inline AdjUnit<EdgeData> &operator[](size_t i)**  
  - 通过索引获取邻接单位。

##### Private Functions
- **inline AdjList(AdjUnit<EdgeData> *begin, AdjUnit<EdgeData> *end)**  
  - 私有构造函数，用于初始化邻接列表。

##### Private Members
- `AdjUnit<EdgeData> *begin_`  
  - 邻接列表的开始指针。

- `AdjUnit<EdgeData> *end_`  
  - 邻接列表的结束指针。

##### Friends
- **friend class OlapBase< EdgeData >**  
  - 允许OlapBase作为朋友类访问内部成员。

- **template<typename EdgeData> struct AdjUnit**  
```cpp
#include <olap_base.h>
```
AdjUnit<EdgeData>表示一个带有EdgeData作为权重类型的相邻边。

**模板参数**  
EdgeData – 边数据的类型。

#### Public Members
- `size_t neighbour`  
  - 邻居的数量或标识符。

- `EdgeData edge_data`  
  - 边的数据。

- **template<> struct AdjUnit<Empty>**  
```cpp
#include <olap_base.h>
```
#### Public Functions
- **template<> union lgraph_api::olap::AdjUnit< Empty >::@4 `__attribute__ ((packed))`**  
  - 对无权连接单位的定义。

#### Public Members
- `size_t neighbour`  
  - 邻居的数量或标识符。

- `Empty edge_data`  
  - 无权边数据。

- **template<typename EdgeData> struct EdgeStringUnit**  
```cpp
#include <olap_base.h>
```
EdgeStringUnit<EdgeData>表示一个带有EdgeData作为权重类型的边，顶点为字符串类型。

**模板参数**  
EdgeData – 边数据的类型。

#### Public Members
- `std::string src`  
  - 源顶点的字符串形式。

- `std::string dst`  
  - 目标顶点的字符串形式。

- `EdgeData edge_data`  
  - 边的数据。

- **template<typename EdgeData> struct EdgeUnit**  
```cpp
#include <olap_base.h>
```
EdgeUnit<EdgeData>表示一个带有EdgeData作为权重类型的边。

**模板参数**  
EdgeData – 边数据的类型。

#### Public Members
- `size_t src`  
  - 源顶点的标识符。

- `size_t dst`  
  - 目标顶点的标识符。

- `EdgeData edge_data`  
  - 边的数据。

- **template<> struct EdgeUnit<Empty>**  
```cpp
#include <olap_base.h>
```
#### Public Functions
- **template<> union lgraph_api::olap::EdgeUnit< Empty >::@6 `__attribute__ ((packed))`**  
  - 对于无权图的连接单位的联合定义。

#### Public Members
- `size_t src`  
  - 源顶点的标识符。

- `size_t dst`  
  - 目标顶点的标识符。

- `Empty edge_data`  
  - 无权边数据。

- **struct Empty**  
```cpp
#include <olap_base.h>
```
Empty用于表示无权图。

- **template<typename EdgeData> class OlapBase**  
```cpp
#include <olap_base.h>
```
AdjList<EdgeData>允许对AdjUnit<EdgeData>进行基于范围的循环。

**Graph**

EdgeData用于表示边的权重（默认类型是Empty，表示无权图）。

**模板参数**  
EdgeData – 边数据的类型。

EdgeData – 图实例表示从txt文件加载的静态（子）图。内部组织使用压缩稀疏矩阵格式，优化了只读访问。

**EdgeData** – 边数据的类型。

**Subclassed by** OlapOnDB< EdgeData >

#### Public Functions

- **inline OlapBase()**  
  - 图的构造函数。

- **inline virtual bool CheckKillThisTask()**  
  - 检查是否需要终止此任务。

- **inline size_t OutDegree(size_t vid)**  
  - 访问某个顶点的出度。
  
  **参数**  
  vid – 要访问的顶点ID（在图中）。

  **返回**  
  指定顶点在图中的出度。

- **inline size_t InDegree(size_t vid)**  
  - 访问某个顶点的入度。

  **参数**  
  vid – 要访问的顶点ID（在图中）。

  **返回**  
  指定顶点在图中的入度。

- **inline AdjList<EdgeData> OutEdges(size_t vid)**  
  - 访问某个顶点的出边。

  **参数**  
  vid – 要访问的顶点ID（在图中）。

  **返回**  
  指定顶点在图中的出边。

- **inline AdjList<EdgeData> InEdges(size_t vid)**  
  - 访问某个顶点的入边。

  **参数**  
  vid – 要访问的顶点ID（在图中）。

  **返回**  
  指定顶点在图中的入边。

- **inline void Transpose()**  
  - 转置图。

- **inline size_t NumVertices()**  
  - 获取图中顶点的数量。

  **返回**  
  图中顶点的数量。

- **inline size_t NumEdges()**  
  - 获取图中边的数量。

  **返回**  
  图中边的数量。

- **template<typename VertexData> inline ParallelVector<VertexData> AllocVertexArray()**  
  - 分配一个类型为VertexData的顶点数组。

  **模板参数**  
  VertexData – 顶点数据的类型。

  **返回**  
  返回一个类型为VertexData的ParallelVector。

- **inline ParallelBitset AllocVertexSubset()**  
  - 分配一个由ParallelBitset表示的顶点子集。

  **返回**  
  返回大小为|V|的图的ParallelBitset。

- **inline void AcquireVertexLock(size_t vid)**  
  - 锁定某个顶点以确保并发更新的正确性。

  **参数**  
  vid – 要锁定的顶点ID（在图中）。

- **inline void ReleaseVertexLock(size_t vid)**
  - Unlock some vertex to ensure correct concurrent updates.  // 解锁某个顶点以确保正确的并发更新。

  **参数**  
  vid – The vertex id (in the Graph) to unlock.  // 要解锁的顶点ID（在图中）。

- **inline VertexLockGuard GuardVertexLock(size_t vid)**
  - Get a VertexLockGuard of some vertex.  // 获取某个顶点的 VertexLockGuard。

  **参数**  
  vid – The vertex id (in the Graph) to lock/unlock.  // 要锁定/解锁的顶点ID（在图中）。

  **返回**  
  A VertexLockGuard corresponding to the specified vertex.  // 与指定顶点对应的 VertexLockGuard。

- **inline bool IfSparse(ParallelBitset &active_vertices)**
  - Judging whether it is sparse mode or dense mode according to the number of vertices.  // 根据顶点的数量判断是稀疏模式还是密集模式。

  **参数**  
  active_vertices – The ParallelBitset of active_vertices.  // 活动顶点的 ParallelBitset。

- **inline void set_num_vertices(size_t vertices)**
  - Assign vertices to the first loaded graph.  // 将顶点分配给第一个加载的图形。

  **参数**  
  vertices – The vertex id (in the Graph) to lock/unlock.  // 要锁定/解锁的顶点ID（在图中）。

- **inline void LoadFromArray(char *edge_array, size_t input_vertices, size_t input_edges, EdgeDirectionPolicy edge_direction_policy)**
  - Load graph data from edge_array.  // 从 edge_array 加载图形数据。

  **参数**  
  edge_array – [in] The data in this array is read into the graph.  // [输入] 此数组中的数据将读取到图形中。  
  input_vertices – The number of vertices in the input graph data.  // 输入图形数据中的顶点数量。  
  input_edges – The number of edges in the input graph data.  // 输入图形数据中的边缘数量。  
  edge_direction_policy – Graph data loading method.  // 图形数据加载方法。

- **template<typename ReducedSum> inline ReducedSum ProcessVertexInRange(std::function<ReducedSum(size_t)> work, size_t lower, size_t upper, ReducedSum zero = 0, std::function<ReducedSum(ReducedSum, ReducedSum)> reduce = reduce_plus<ReducedSum>)**
  - Execute a parallel-for loop in the range [lower, upper).  // 在区间 [lower, upper) 执行并行 for 循环。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 当发生运行时错误条件时引发。

  **模板参数**  
  ReducedSum – Type of the reduced sum.  // 减少总和的类型。

  **参数**  
  work – The function describing the work.  // 描述工作的函数。  
  lower – The lower bound of the range (inclusive).  // 范围的下界（包含）。  
  upper – The upper bound of the range (exclusive).  // 范围的上界（不包含）。  
  zero – (Optional) The initial value for reduction.  // （可选）用于缩减的初始值。  
  reduce – (Optional) The function describing the reduction logic.  // （可选）描述减少逻辑的函数。

  **返回**  
  A reduction value.  // 一个减少值。

- **template<typename ReducedSum, typename Algorithm> inline ReducedSum ProcessVertexInRange(std::function<ReducedSum(Algorithm, size_t)> work, size_t lower, size_t upper, Algorithm algorithm, ReducedSum zero = 0, std::function<ReducedSum(ReducedSum, ReducedSum)> reduce = reduce_plus<ReducedSum>)**

- **template<typename ReducedSum> inline ReducedSum ProcessVertexActive(std::function<ReducedSum(size_t)> work, ParallelBitset &active_vertices, ReducedSum zero = 0, std::function<ReducedSum(ReducedSum, ReducedSum)> reduce = reduce_plus<ReducedSum>)**
  - Process a set of active vertices in parallel.  // 并行处理一组活动顶点。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 当发生运行时错误条件时引发。

  **模板参数**  
  ReducedSum – Type of the reduced sum.  // 减少总和的类型。

  **参数**  
  work – The function describing each vertex’s work.  // 描述每个顶点工作的函数。  
  active_vertices – [inout] The active vertex set.  // [输入输出] 活动顶点集。  
  zero – (Optional) The initial value for reduction.  // （可选）用于缩减的初始值。  
  reduce – (Optional) The function describing the reduction logic.  // （可选）描述减少逻辑的函数。

  **返回**  
  A reduction value.  // 一个减少值。

- **template<typename ReducedSum, typename Algorithm> inline ReducedSum ProcessVertexActive(std::function<ReducedSum(Algorithm, size_t)> work, ParallelBitset &active_vertices, Algorithm algorithm, ReducedSum zero = 0, std::function<ReducedSum(ReducedSum, ReducedSum)> reduce = reduce_plus<ReducedSum>)**

#### Protected Functions

- **inline virtual void Construct()**  // 构造函数。

#### Protected Attributes

- `size_t num_vertices_`  // 顶点数量。
- `size_t num_edges_`  // 边的数量。
- `size_t edge_data_size_`  // 边数据大小。
- `size_t adj_unit_size_`  // 邻接单元大小。
- `size_t edge_unit_size_`  // 边单元大小。
- `EdgeDirectionPolicy edge_direction_policy_`  // 边方向策略。
- `EdgeUnit<EdgeData> *edge_list_`  // 边列表。
- `ParallelVector<size_t> out_degree_`  // 出度。
- `ParallelVector<size_t> in_degree_`  // 入度。
- `ParallelVector<size_t> out_index_`  // 出索引。
- `ParallelVector<size_t> in_index_`  // 入索引。
- `ParallelVector<AdjUnit<EdgeData>> out_edges_`  // 出边。
- `ParallelVector<AdjUnit<EdgeData>> in_edges_`  // 入边。
- `ParallelVector<bool> lock_array_`  // 锁数组。

#### class ParallelBitset

```cpp
#include <olap_base.h>
```
ParallelBitset implements the concurrent bitset data structure, which is usually used to represent active vertex sets.  // ParallelBitset 实现了并发位集数据结构，通常用于表示活动顶点集。

##### Public Functions

- **explicit ParallelBitset(size_t size)**
  - Construct a ParallelBitset.  // 构造一个 ParallelBitset。

  **参数**  
  size – The size of the bitset (i.e. the number of bits).  // 位集的大小（即位数）。

- **ParallelBitset(const ParallelBitset &rhs) = delete**  // 禁止复制构造函数。

- **inline ParallelBitset &operator=(ParallelBitset &&rhs)**  // 移动赋值运算符。

- **inline ParallelBitset(ParallelBitset &&rhs)**  // 移动构造函数。

- **inline ParallelBitset()**  // 默认构造函数。

- **~ParallelBitset()**  // 析构函数。

- **void Clear()**
  - Clear the bitset.  // 清除位集。

- **void Fill()**
  - Fill the bitset.  // 填充位集。

- **bool Has(size_t i)**
  - Test a specified bit.  // 测试指定的位。

  **参数**  
  i – The bit to test.  // 要测试的位。

  **返回**  
  Whether the bit is set or not.  // 位是否被设置。

- **bool Add(size_t i)**
  - Set a specified bit.  // 设置指定的位。

  **参数**  
  i – The bit to set.  // 要设置的位。

  **返回**  
  Whether the operation is a true addition or not.  // 操作是否是真正的添加。

- **void Swap(ParallelBitset &other)**
  - Swap the current bitset with another one.  // 将当前位集与另一个位集交换。

  **参数**  
  other – [inout] The other bitset to swap with.  // [输入输出] 另一个待交换的位集。

- **inline uint64_t *Data()**  // 获取数据指针。

- **inline size_t Size()**  // 获取当前大小。

##### Private Members

- `uint64_t *data_`  // 数据指针。
- `size_t size_`  // 大小。

---

#### template<typename T> class ParallelVector

```cpp
#include <olap_base.h>
```
ParallelVector<T> aims to mimic std::vector<T>. Note that the deletions other than clearing are not supported.  // ParallelVector<T> 旨在模拟 std::vector<T>。请注意，除了清除外，不支持其他删除操作。

##### Public Functions

- **inline explicit ParallelVector(size_t capacity)**
  - Construct a ParallelVector<T>.  // 构造一个 ParallelVector<T>。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 当发生运行时错误条件时引发。

  **参数**  
  capacity – The capacity of the vector.  // 向量的容量。

- **inline explicit ParallelVector(size_t capacity, size_t size)**
  - Construct a ParallelVector<T>.  // 构造一个 ParallelVector<T>。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 当发生运行时错误条件时引发。

  **参数**  
  capacity – The capacity of the vector.  // 向量的容量。  
  size – The initial size of the vector.  // 向量的初始大小。

- **inline ParallelVector(T *data, size_t size)**
  - Construct a ParallelVector<T>.  // 构造一个 ParallelVector<T>。

  **参数**  
  data – The initial data of the vector.  // 向量的初始数据。  
  size – The initial size of the vector. And the initial capacity equals initial size.  // 向量的初始大小，初始容量等于初始大小。

- **ParallelVector(const ParallelVector<T> &rhs) = delete**  // 禁止复制构造函数。

- **inline ParallelVector(ParallelVector<T> &&rhs)**  // 移动构造函数。

- **inline ParallelVector<T> &operator=(ParallelVector<T> &&rhs)**  // 移动赋值运算符。

- **inline ParallelVector()**
  - Default constructor of ParallelVector<T>.  // ParallelVector<T> 的默认构造函数。

- **inline void Destroy()**
  - Destroy ParallelVector<T>.  // 销毁 ParallelVector<T>。

- **inline ~ParallelVector()**  // 析构函数。

- **inline T &operator[](size_t i)**  // 重载下标运算符。

- **inline T *begin()**  // 返回开始迭代器。

- **inline T *end()**  // 返回结束迭代器。

- **inline T &Back()**  // 返回最后一个元素。

- **inline T *Data()**  // 返回数据指针。

- **inline size_t Size()**  // 返回当前大小。

- **inline size_t Capacity()**  // 返回容量。

- **inline bool Destroyed()**  // 检查是否被销毁。

- **inline void Resize(size_t size)**
  - Change ParallelVector size. Note the new size should be larger than or equal to elder size.  // 改变 ParallelVector 的大小。注意新大小应大于或等于旧大小。

  **参数**  
  size – Value of new size.  // 新大小的值。

- **inline void Resize(size_t size, const T &elem)**
  - Change ParallelVector size and initialize the new element with elem. Note the new size should be larger than or equal to elder size.  
  - 改变 ParallelVector 的大小，并用 elem 初始化新元素。注意新大小应该大于或等于旧大小。

  **参数**  
  size – Value of new size.  // 新大小的值  
  elem – Initial value of new-added element.  // 新添加元素的初始值

- **inline void Clear()**
  - Clear all data and change size to 0.  
  - 清除所有数据并将大小设置为 0。

- **inline void ReAlloc(size_t capacity)**
  - Destroy elder data and allocate with new capacity.  
  - 销毁旧数据并分配新的容量。

  **参数**  
  capacity – New capacity value.  // 新的容量值

- **inline void Fill(T elem)**
  - Assign the vector’s elements with a common value.  
  - 将向量的元素赋值为一个公共值。

  **参数**  
  elem – The common value.  // 公共值

  This action is performed in parallel, so you should not call it inside another parallel region (via Worker::Delegate).  
  - 该操作是并行执行的，因此您不应在另一个并行区域内部调用它（通过 Worker::Delegate）。

- **inline void Append(const T &elem, bool atomic = true)**
  - Append an element to the vector.  
  - 将一个元素附加到向量中。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 在发生运行时错误条件时引发。

  **参数**  
  elem – The element.  // 元素  
  atomic – (Optional) Whether atomic instructions should be used or not.  // （可选）是否使用原子指令。

- **inline void Append(T *buf, size_t count, bool atomic = true)**
  - Append an array of elements to the vector.  
  - 将一组元素附加到向量中。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 在发生运行时错误条件时引发。

  **参数**  
  buf – [inout] The array pointer.  // [输入输出] 数组指针  
  count – The array length.  // 数组长度  
  atomic – (Optional) True to atomic.  // （可选）是否为真以使用原子操作。

- **inline void Append(ParallelVector<T> &other, bool atomic = true)**
  - Append another vector of elements to this.  
  - 将另一个元素向量附加到此向量。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.  // 在发生运行时错误条件时引发。

  **参数**  
  other – [inout] The other vector.  // [输入输出] 另一个向量  
  atomic – (Optional) True to atomic.  // （可选）是否为真以使用原子操作。

- **inline void Swap(ParallelVector<T> &other)**
  - Swap the current vector with another one.  
  - 将当前向量与另一个向量交换。

  **参数**  
  other – [inout] The other vector to swap with.  // [输入输出] 要交换的另一个向量

- **inline ParallelVector<T> Copy()**
  - Copy the current vector.  
  - 复制当前向量。

  **返回**  
  A new vector with the same copied content.  // 一个新的向量，具有相同的复制内容。

##### Private Members

- `bool destroyed_`
- `size_t capacity_`
- `T *data_`
- `size_t size_`

---

#### struct ThreadState

```cpp
#include <olap_base.h>
```

##### Public Members

- `size_t curr`
- `size_t end`
- `int state`

---

#### class VertexLockGuard

```cpp
#include <olap_base.h>
```
The VertexLockGuard automatically acquires the lock on construction and releases the lock on destruction.  
- VertexLockGuard是一个机制，用于控制程序对点数据的访存权限。

##### Public Functions

- **explicit VertexLockGuard(volatile bool *lock)**

- **VertexLockGuard(const VertexLockGuard &rhs) = delete**

- **VertexLockGuard(VertexLockGuard &&rhs) = default**

- **~VertexLockGuard()**

##### Private Members

- `bool *lock_`

---

#### class Worker

```cpp
#include <olap_base.h>
```
All the parallel tasks should be delegated through Worker to prevent a huge number of threads being populated via OpenMP.  
- 所有并行任务应通过 Worker 委托，以防止通过 OpenMP 创建大量线程。

##### Public Functions

- **Worker()**

- **~Worker()**

- **void Delegate(const std::function<void()> &work)**
  - Send some work to the Worker instance.  
  - 向 Worker 实例发送一些工作。

  **参数**  
  work – The function describing the work.  // 描述工作的函数

  Exceptions can be thrown in the work function if necessary. Note that Delegate cannot be nested.  
  - 如果需要，可以在工作函数中抛出异常。请注意，Delegate 不能嵌套。

- **template<typename Compute> inline void DelegateCompute(const std::function<void(Compute&)> &work, Compute &compute)**

##### Public Static Functions

- **static std::shared_ptr<Worker> &SharedWorker()**
  - Get the global (shared) worker.  
  - 获取全局（共享） Worker。

  **返回**  
  A shared pointer to the global Worker instance.  // 指向全局 Worker 实例的共享指针。

##### Private Members

- `bool stopping_`
- `bool has_work_`
- `std::mutex mutex_`
- `std::condition_variable cv_`
- `std::shared_ptr<std::packaged_task<void()>> task_`
- `std::thread worker_`














## olap_on_db

TuGraph OLAP interface. To implement a plugin that performs graph analytics on TuGraph, users can load a Snapshot from the database and then use the Gather-Apply-Scatter style interface to do the computation.

TuGraph OLAP 接口。要实现一个在 TuGraph 上执行图分析的插件，用户可以从数据库加载一个快照，然后使用 Gather-Apply-Scatter 风格的接口进行计算。

```cpp
namespace lgraph_api
namespace olap
```

### Variables

- **static constexpr size_t SNAPSHOT_PARALLEL = 1ul << 0**
  - The available options for (graph construction) flags.
  # （图构建）标志的可用选项。

- **static constexpr size_t SNAPSHOT_UNDIRECTED = 1ul << 1**
  # 无向图的标志。

- **static constexpr size_t SNAPSHOT_IDMAPPING = 1ul << 2**
  # ID映射的标志。

- **static constexpr size_t SNAPSHOT_OUT_DEGREE = 1ul << 3**
  - The following options are not implemented yet.
  # 以下选项尚未实现。

- **static constexpr size_t SNAPSHOT_IN_DEGREE = 1ul << 4**
  # 入度的标志。

- **static constexpr size_t SNAPSHOT_OUT_EDGES = 1ul << 5**
  # 出边的标志。

- **static constexpr size_t SNAPSHOT_IN_EDGES = 1ul << 6**
  # 入边的标志。

- **template<typename EdgeData> std::function<bool(OutEdgeIterator&, EdgeData&)> edge_convert_default = [](OutEdgeIterator &eit, EdgeData &edge_data) -> bool { edge_data= 1;return true;}**
  - Default Parser for weighted edges for graph.
  # 图的加权边的默认解析器。

  **Return**  
  Edge is converted into graph or not.
  # 边是否被转换为图。

- **template<typename EdgeData> std::function<bool(OutEdgeIterator&, EdgeData&)> edge_convert_weight = [](OutEdgeIterator &eit, EdgeData &edge_data) -> bool { edge_data= eit.GetField("weight").real();return true;}**
  - Example parser for extracting from edge property “weight”
  # 用于从边属性“权重”中提取的示例解析器。

  **Return**  
  Edge is converted into graph or not.
  # 边是否被转换为图。

- **template<typename EdgeData> class OlapOnDB : public lgraph_api::olap::OlapBase<EdgeData>**
  
  ```cpp
  #include <olap_on_db.h>
  ```

  Snapshot is a derived class of Graph. Snapshot instances represent static (sub)graphs exported from LightningGraph. The internal organization uses compressed sparse matrix formats which are optimized for read-only accesses.
  # Snapshot 是图的派生类。 Snapshot 实例表示从 LightningGraph 导出的静态（子）图。内部组织使用针对只读访问优化的压缩稀疏矩阵格式。

  EdgeData is used for representing edge weights (the default type is Empty which is used for unweighted graphs).
  # EdgeData 用于表示边权重（默认类型是 Empty，适用于无权图）。

  **模板参数**  
  EdgeData – Type of the edge data.
  # EdgeData – 边数据的类型。

### Public Functions

- **inline OlapOnDB(GraphDB *db, Transaction &txn, size_t flags = 0, std::function<bool(VertexIterator&)> vertex_filter = nullptr, std::function<bool(OutEdgeIterator&, EdgeData&)> out_edge_filter = nullptr)**
  - Generate a graph with LightningGraph. For V1/V2 Procedures
  # 使用 LightningGraph 生成图。用于 V1/V2 过程。

- **inline OlapOnDB(GraphDB &db, Transaction &txn, size_t flags = 0, std::function<bool(VertexIterator&)> vertex_filter = nullptr, std::function<bool(OutEdgeIterator&, EdgeData&)> out_edge_filter = nullptr)**
  - Generate a graph with LightningGraph. For V1 Procedures
  # 使用 LightningGraph 生成图。用于 V1 过程。

  **Note**: Read-write transactions are not recommended here for safety, e.g., some vertices might be removed causing inconsistencies in the analysis, and vertex data extraction may not work for deleted vertices. The constructed graph should contain all vertices whose vertex_filter calls return true and all edges sourced from these vertices whose out_edge_filter calls return true. If SNAPSHOT_UNDIRECTED is specified, the graph will be made symmetric (i.e., reversed edges are also added to the graph).
  # 注意：这里不推荐使用读写事务以确保安全，例如，某些顶点可能会被删除，从而导致分析中的不一致性，并且顶点数据提取可能无法处理已删除的顶点。构建的图应该包含所有 vertex_filter 调用返回 true 的顶点，以及所有出自这些顶点的 out_edge_filter 调用返回 true 的边。如果指定了 SNAPSHOT_UNDIRECTED，则图将变为对称（即反向边也将被添加到图中）。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.
  # std::runtime_error – 当发生运行时错误条件时抛出。

  **参数**  
  db – [inout] The GraphDB instance.  
  txn – [inout] The transaction.  
  flags – (Optional) The generation flags.  
  vertex_filter – [inout] (Optional) A function filtering vertices.  
  out_edge_filter – [inout] (Optional) A function filtering out edges.

- **inline OlapOnDB(GraphDB &db, Transaction &txn, std::vector<std::vector<std::string>> label_list, size_t flags = 0)**

- **inline OlapOnDB(Transaction &txn, size_t flags = 0, std::function<bool(VertexIterator&)> vertex_filter = nullptr, std::function<bool(OutEdgeIterator&, EdgeData&)> out_edge_filter = nullptr)**
  - Generate a graph without LightningGraph. For V2 Procedures
  # 在没有 LightningGraph 的情况下生成图。用于 V2 过程。

  **抛出**  
  std::runtime_error – Raised when a runtime error condition occurs.
  # std::runtime_error – 当发生运行时错误条件时抛出。

  **参数**  
  txn – [inout] The transaction.  
  flags – (Optional) The generation flags.  
  vertex_filter – [inout] (Optional) A function filtering vertices.  
  out_edge_filter – [inout] (Optional) A function filtering out edges.

- **OlapOnDB() = delete**

- **OlapOnDB(const OlapOnDB<EdgeData> &rhs) = delete**

- **OlapOnDB(OlapOnDB<EdgeData> &&rhs) = default**

- **inline OlapOnDB<EdgeData> &operator=(OlapOnDB<EdgeData> &&rhs)**

- **inline virtual ~OlapOnDB()**

- **template<typename VertexData> inline ParallelVector<VertexData> ExtractVertexData(std::function<void(VertexIterator&, VertexData&)> extract)**
  - Extract a vertex array from the graph.
  # 从图中提取顶点数组。

  **参数**  
  extract – The function describing the extraction logic.
  # extract – 描述提取逻辑的函数。

  **返回**  
  A ParallelVector containing each vertex’s extracted data.
  # 一个包含每个顶点提取数据的 ParallelVector。

- **template<typename VertexData> inline void WriteToFile(ParallelVector<VertexData> &vertex_data, const std::string &output_file, std::function<bool(size_t vid, VertexData &vdata)> output_filter = nullptr)**
  - Write vertex data to a file.
  # 将顶点数据写入文件。

  **参数**  
  vertex_data – The parallel vector storing the vertex data.  
  output_file – The path to the output file.
  # vertex_data – 存储顶点数据的并行向量。  
  # output_file – 输出文件的路径。

- **template<typename VertexData> inline void WriteToFile(bool detail_output, ParallelVector<VertexData> &vertex_data, const std::string &output_file, std::function<bool(size_t vid, VertexData &vdata)> output_filter = nullptr)**
  - Write vertex data (include label, primary_field, field_data) to a file.
  # 将顶点数据（包括标签、主字段、字段数据）写入文件。

  **参数**  
  detail_output – always true  
  vertex_data – The parallel vector storing the vertex data.  
  output_file – The path to the output file.
  # detail_output – 始终为 true  
  # vertex_data – 存储顶点数据的并行向量。  
  # output_file – 输出文件的路径。

- **template<typename VertexData> inline void WriteToGraphDB(ParallelVector<VertexData> &vertex_data, const std::string &vertex_field)**
  - Write vertex data to the graph database.
  # 将顶点数据写入图数据库。

  **参数**  
  vertex_data – [inout] The parallel vector storing the vertex data.  
  vertex_field – [in] The name of the vertex field.
  # vertex_data – [inout] 存储顶点数据的并行向量。  
  # vertex_field – [in] 顶点字段的名称。

- **inline int64_t OriginalVid(size_t vid)**
  - Get the original vertex id (in LightningGraph) of some vertex.
  # 获取某个顶点的原始顶点 ID（在 LightningGraph 中）。

  **参数**  
  vid – The vertex id (in the graph) to access.
  # vid – 要访问的顶点 ID（在图中）。

  **返回**  
  The original id of the specified vertex in the graph.
  # 指定顶点在图中的原始 ID。

- **inline size_t MappedVid(size_t original_vid)**
  - Get the mapped vertex id (in the graph) of some vertex.
  # 获取某个顶点的映射顶点 ID（在图中）。

  **参数**  
  original_vid – The original vertex id (in LightningGraph) to access.
  # original_vid – 要访问的原始顶点 ID（在 LightningGraph 中）。

  **返回**  
  The mapped id of the specified vertex (in LightningGraph).
  # 指定顶点（在 LightningGraph 中）的映射 ID。

- **inline Transaction &GetTransaction()**

### Private Functions

- **inline void Init(size_t num_vertices)**

- **inline virtual bool CheckKillThisTask()**
  - This decision formula is used to determine whether to stop the algorithm running in OlapOnDB.
  # 此决策公式用于确定是否停止在 OlapOnDB 中运行的算法。

- **inline virtual void Construct()**

- **inline void ConstructWithVid()**

- **inline void ConstructWithDegree()**

### Private Members

- `GraphDB *db_`
- `Transaction &txn_`
- `ParallelVector<size_t> original_vids_`
- `cuckoohash_map<size_t, size_t> vid_map_`
- `size_t flags_`
- `std::function<bool(VertexIterator&)> vertex_filter_`
- `std::function<bool(OutEdgeIterator&, EdgeData&)> out_edge_filter_`












## olap_profile

```cpp
namespace lgraph_api
namespace olap
```
在这里定义了 `lgraph_api` 和 `olap` 命名空间，便于对相应的功能进行组织和管理。

### class MemUsage

```cpp
#include <olap_profile.h>
```
该行包含了 `olap_profile.h` 头文件，这通常包含了 `MemUsage` 类的声明和其他相关的定义。

### Public Functions

- **inline MemUsage()**
  - 构造函数，用于初始化 `MemUsage` 类的实例。

- **inline int64_t getMaxMemUsage()**
  - 获取最大的内存使用量，返回类型为64位整数。

- **void reset()**
  - 重置内存使用记录，将已记录的内存信息清零。

- **void startMemRecord(unsigned int interval = 1000)**
  - 开始内存使用记录，接受一个参数 `interval`（默认1000毫秒），用来指定记录频率。

- **void print()**
  - 打印内存使用记录的详细信息，方便用户查看内存情况。

### Private Functions

- **int parseMemLine(char *line)**
  - 解析内存使用的文本行，返回解析得到的内存值，通常用于将字符串转换为数字。

### Private Members

- `size_t maxMemUsage`
  - 私有成员变量，用于存储最大的内存使用量，类型为 `size_t`，通常用于表示存储大小。
