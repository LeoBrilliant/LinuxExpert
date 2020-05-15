# C++
## Extract number from a string
- https://stackoverflow.com/questions/30073839/c-extract-number-from-the-middle-of-a-string
sscanf( ex.c_str(), "%*[^_]_%d", &num ); */

## template
- https://stackoverflow.com/questions/1762049/templated-operator-overload-c 语法
- https://stackoverflow.com/questions/1116654/function-template-with-an-operator 语法
- http://blog.csdn.net/caroline_wendy/article/details/14222295 给出了实现要放在声明之后的提示
- https://stackoverflow.com/questions/6988924/invalid-use-of-incomplete-type-forward-declaration 不是很有用
- https://stackoverflow.com/questions/19962812/error-member-access-into-incomplete-type-forward-declaration-of 真正能解决问题
- http://en.cppreference.com/w/cpp/language/template_parameters C++参考
- https://stackoverflow.com/questions/11191447/strategy-pattern-in-c-with-template 策略模式
- http://www.bogotobogo.com/DesignPatterns/strategy.php 策略模式

## type_info
- http://www.cplusplus.com/reference/typeinfo/type_info/name/ typeid( variable ).name()

## friend
- https://stackoverflow.com/questions/18959276/making-a-template-parameter-as-friend-of-template-class
- https://stackoverflow.com/questions/6510041/template-parameter-as-a-friend
- https://stackoverflow.com/questions/702650/making-a-template-parameter-a-friend
template< typename T > class Base
{
	friend T; // Not friend class T;
};

## Pointer
- https://stackoverflow.com/questions/551069/testing-pointers-for-validity-c-c at present, we can't check the pointer's validity when it is not null.
- https://stackoverflow.com/questions/1576300/checking-if-a-pointer-is-allocated-memory-or-not

## boost
### shared_ptr
- shared_ptr is initialized to 0(boost::none) in default constructor.
- We can set the shared_ptr to null by .reset(), = nullPtr, or = nullptr.
- https://stackoverflow.com/questions/621220/null-pointer-with-boostshared-ptr

### property_tree
- output property_tree as a string
std::stringstream ss;
boost::property_tree::json_parser::write_json(ss, pt);
std::cout << ss.str() << std::endl;

- output everything as a string https://stackoverflow.com/questions/2855741/why-boost-property-tree-write-json-saves-everything-as-string-is-it-possible-to

- read json from a string
  std::stringstream ss;
  ss << argv[1];

  boost::property_tree::ptree pt;
  boost::property_tree::read_json(ss, pt);
  std::cout << pt.get<std::string>("foo") << std::endl;
- https://stackoverflow.com/questions/21537637/c-boost-parse-dynamically-generated-json-string-not-a-file
- https://stackoverflow.com/questions/31345401/parse-json-array-as-stdstring-with-boost-ptree

- property_tree output non-string types as string
- https://stackoverflow.com/questions/29243662/how-to-get-boost-json-to-use-the-correct-data-types
- https://stackoverflow.com/questions/2855741/why-boost-property-tree-write-json-saves-everything-as-string-is-it-possible-to

### mutex
- boost::mutex::~mutex(): Assertion `!pthread_mutex_destroy(&m)' failed
because the mutex is locked when destruct.

### program_options
undefined reference to 
add some flags to compilation command line -lboost_program_options -L/usr/lib/x86_64-linux-gnu
- https://stackoverflow.com/questions/12179154/undefined-reference-to-boostprogram-optionsoptions-descriptionm-default-l/12179310

## filesystem
- must link with -lboost_filesystem, cause it's not a header only library.
- https://stackoverflow.com/questions/7972314/c-boostfilesystem-undefined-reference-to-boostfilesystem3pathroot-nam
- https://stackoverflow.com/questions/5677652/how-can-i-change-the-current-path-using-boost-filesystem Change current path current_path( new_path )
- https://stackoverflow.com/questions/3485166/change-the-current-working-directory-in-c chdir()
- https://stackoverflow.com/questions/30029343/how-do-i-create-a-file-with-boost-filesystem-without-opening-it create_directory()
- https://stackoverflow.com/questions/2203919/boostfilesystem-exists-on-directory-path-fails-but-is-directory-is-ok exists() is_directory()
- 

### undefined reference to `boost::filesystem::detail::copy_file`
```
#define BOOST_NO_CXX11_SCOPED_ENUMS
#include <boost/filesystem.hpp>
#undef BOOST_NO_CXX11_SCOPED_ENUMS
```
- https://stackoverflow.com/questions/35007134/c-boost-undefined-reference-to-boostfilesystemdetailcopy-file

### accumulator
- http://www.boost.org/doc/libs/1_63_0/doc/html/accumulators/user_s_guide.html

## string
			  
- find and replace https://stackoverflow.com/questions/3418231/replace-part-of-a-string-with-another-string
- https://stackoverflow.com/questions/5878775/how-to-find-and-replace-string
Try a combination of std::string::find and std::string::replace.

This gets the position:

size_t f = s.find("text to replace");
And this replaces the text:

s.replace(f, std::string("text to replace").length(), "new text");
Now you can simply create a function for your convenience:

std::string myreplace(std::string &s,
                      const std::string &toReplace,
                      const std::string &replaceWith)
{
    return(s.replace(s.find(toReplace), toReplace.length(), replaceWith));
}

- multiline string https://stackoverflow.com/questions/1135841/c-multiline-string-literal

## file 
- remove http://en.cppreference.com/w/cpp/io/c/remove
- remove http://www.cplusplus.com/reference/cstdio/remove/
- input and output http://www.cplusplus.com/doc/tutorial/files/

## function
- https://stackoverflow.com/questions/15760929/how-to-clear-boost-function function.clear() funtion.empty()

std::lower_bound

## Network
### multicast
- http://www.tenouk.com/Module41c.html
- http://ntrg.cs.tcd.ie/undergrad/4ba2/multicast/antony/example.html
- https://web.cs.wpi.edu/~claypool/courses/4514-B99/samples/multicast.c
sender just needs to create a socket, then send messages to the multicast ip addr and port.
receiver create a socket, bind to the multicast port, then use setsocketopt to join the multicast group, if success, it can receive messages.
try to set the port reuseable.

# TBB
## compare and swap
value_type compare_and_swap( value_type new_value, value_type comparand )
Let x be the value of *this. Atomically compares x with comparand, and if they are equal, sets x=new_value.
Returns: Original value of x.
- https://software.intel.com/en-us/node/506277

# Linux
## gettimeofday
struct timeval tv;

gettimeofday(&tv, NULL);

unsigned long long millisecondsSinceEpoch =
    (unsigned long long)(tv.tv_sec) * 1000 +
    (unsigned long long)(tv.tv_usec) / 1000;
- https://stackoverflow.com/questions/1952290/how-can-i-get-utctime-in-millisecond-since-january-1-1970-in-c-language

## clock_gettime
struct timespec spec;

clock_gettime(CLOCK_REALTIME, &spec);
- https://stackoverflow.com/questions/3756323/getting-the-current-time-in-milliseconds

## fnmatch
- http://man7.org/linux/man-pages/man3/fnmatch.3.html

## open file failed
because the file doesn't exist when open invoked. Do as below:
fd = open(tmpname, O_WRONLY | O_APPEND | O_CREAT, 0644);
- https://stackoverflow.com/questions/28466715/using-open-to-create-a-file-in-c
- https://stackoverflow.com/questions/2395465/create-a-file-in-linux-using-c

# expected type-specifier before 'ClassName'
check include or namespace.
- https://stackoverflow.com/questions/8845117/error-expected-type-specifier-before-classname

# stringstream 
## std::stringstream.str().c_str() points to NULL
std::stringstream.str() returns a temporary object of type std::string. This temporary goes out of scope when TLStream::c_str() returns, leaving the returned char const* pointer pointing to freed memory.
std::string const& str = out.str();
char const* intString = str.c_str();

- https://stackoverflow.com/questions/11164982/stdostringstream-isnt-returning-a-valid-string
- https://stackoverflow.com/questions/10642253/stringstream-c-str-corruption-in-c
- https://stackoverflow.com/questions/16594298/why-stringstreamstr-truncates-string

# Comparision of tow floating point values
if (fabs(x-y) < K * FLT_EPSILON * fabs(x+y))
CPPUNIT_ASSERT_DOUBLES_EQUAL(0.364 * 10220, sym_data.position, FP_EPSILON);
- https://stackoverflow.com/questions/10334688/how-dangerous-is-it-to-compare-floating-point-values

# Strange linking error: DSO missing from command line
在链接的时候，动态库或者静态库出问题了。下面的链接也只是给出了初步的解决方案，通用性不强。主要还是要解决版本依赖的问题，使用正确的库。
- https://stackoverflow.com/questions/19901934/strange-linking-error-dso-missing-from-command-line

# Can't static_cast from double * to int *
static_cast means that a pointer of the source type can be used as a pointer of the destination type, which requires a subtype relationship.
we can try 
r = reinterpret_cast<int *> (p);

- https://stackoverflow.com/questions/2473628/c-cant-static-cast-from-double-to-int

# Friend function of a private inner class
class Outer
{
    class Inner
    {
        int data;

        friend swap(Inner& lhs, Inner& rhs);     
    }
    friend swap(Inner& lhs, Inner& rhs);
}

void swap(Outer::Inner& lhs, Outer::Inner& rhs)
{
    using std::swap;
    swap(lhs.data, rhs.data);
}

- https://stackoverflow.com/questions/22453192/friend-function-of-a-private-inner-class

# Null Statement
#define NULL_STATEMENT
#define LOG_DEBUG( fmt, ... ) \
    NULL_STATEMENT

- https://stackoverflow.com/questions/40440400/what-is-a-null-statement-in-c

# variadic macro
#define FOO(fmt, ...) printf(fmt, ##__VA_ARGS__)
#define debug(...) printf(__VA_ARGS__)

- https://stackoverflow.com/questions/679979/how-to-make-a-variadic-macro-variable-number-of-arguments
- https://gcc.gnu.org/onlinedocs/cpp/Variadic-Macros.html
- http://www.cnblogs.com/alexshi/archive/2012/03/09/2388453.html

# Exceptions and Error Handling
- https://isocpp.org/wiki/faq/exceptions

# cppreference
- http://en.cppreference.com/w/cpp/language/lifetime
- http://en.cppreference.com/w/cpp/language/new
- http://en.cppreference.com/w/cpp/memory/new/operator_new

# gcc -fmessage-length=n
-fmessage-length=n
       Try to format error messages so that they fit on lines of about n characters.  The default is 72 characters for g++ and 0 for the rest of the front ends supported by GCC.  If n is
       zero, then no line-wrapping will be done; each error message will appear on a single line.
- https://stackoverflow.com/questions/12015624/what-is-meanging-of-gcc-option-fmessage-length
- man gcc

# tbb
## Atomic Operations
x.compare_and_swap(y,z)

if x equals z, then do x=y. In either case, return old value of x.
if x doesn't equals z, then do nothing. return the old value of x.

- https://www.threadingbuildingblocks.org/docs/help/index.htm#tbb_userguide/Design_Patterns/Compare_and_Swap_Loop.html

# CPPUNIT
# setUp() and tearDown()
The methods wrap each individual test case.
- https://stackoverflow.com/questions/10302054/cppunit-setup-and-teardown
- http://cppunit.sourceforge.net/doc/1.8.0/class_cpp_unit_1_1_test_fixture.html

# C++ deprecated conversion from string constant to 'char*'  */
use const char * instead of char *
- https://stackoverflow.com/questions/1524356/c-deprecated-conversion-from-string-constant-to-char

# std::string formatting like sprintf
char buff[100];
snprintf(buff, sizeof(buff), "%s", "Hello");
std::string buffAsStdStr = buff;
- https://stackoverflow.com/questions/2342162/stdstring-formatting-like-sprintf

# shared_ptr
## make_shared vs new

std::shared_ptr<Object> p1 = std::make_shared<Object>("foo"); // just one memory allocation, exception-safety, especially when passing more then one new-created shared_ptrs.
std::shared_ptr<Object> p2(new Object("foo")); // two memory allocations.

- https://stackoverflow.com/questions/20895648/difference-in-make-shared-and-normal-shared-ptr-in-c # detailed explanation
- https://stackoverflow.com/questions/18301511/stdshared-ptr-initialization-make-sharedfoo-vs-shared-ptrtnew-foo # for short explanation

# vector
## appending a vector to another # concatenating two std::vectors
a.insert(a.end(), b.begin(), b.end());
a.insert(std::end(a), std::begin(b), std::end(b));
std::copy(b.begin(), b.end(), std::back_inserter(a));

- https://stackoverflow.com/questions/2551775/appending-a-vector-to-a-vector
- https://stackoverflow.com/questions/201718/concatenating-two-stdvectors

## move elements from src to dst
dst.insert(dst.end(), std::make_move_itertator(src.begin()), std::make_move_iterator(src.end()));
- https://stackoverflow.com/questions/201718/concatenating-two-stdvectors

# iterator invalidation
- https://stackoverflow.com/questions/20164109/avoiding-iterator-invalidation-using-indices-maintaining-clean-interface
- https://stackoverflow.com/questions/16904454/what-is-iterator-invalidation
- https://stackoverflow.com/questions/6438086/iterator-invalidation-rules/11336379#11336379

# offsetof
- #include<stddef.h>
- #define offsetof(type, member) (size_t)&(((type*)0)->member)
- //*
- https://blog.csdn.net/hellolingyun/article/details/37603351

# GCC
## undefined reference to xxx
I have put the library path and name to the g++ command line, but still got the error message.
$ g++ -Iinclude -Llib -llibname main.cpp
Though I tried the following command
$ g++ -Iinclude -Llib liblibname.so main.cpp
The same error messages appeared.
I am sure that the 'undefined symbol' is in the shared object file.
$ nm liblibname.so | c++filt | grep symbolname
Finally, I found when I use this, things go fine.
$ g++ -Iinclude -Llib main.cpp -llibname

- baidu helps at some degree...
- https://blog.csdn.net/lovemysea/article/details/79520516
- https://blog.csdn.net/aiwoziji13/article/details/7330333

# C++

## priority_queue
用 std::greater<T> 将导致最小元素作为 top() 出现。
- [std::priority_queue - cppreference.com](https://zh.cppreference.com/w/cpp/container/priority_queue)
## Catch2
- [catch2:一个好用的C++单元测试框架 - gigglesun的专栏 - CSDN博客](https://blog.csdn.net/ithiker/article/details/87909651)
- [Catch2/tutorial.md at master · catchorg/Catch2 · GitHub](https://github.com/catchorg/Catch2/blob/master/docs/tutorial.md#top)
- [Catch2/test-cases-and-sections.md at master · catchorg/Catch2 · GitHub](https://github.com/catchorg/Catch2/blob/master/docs/test-cases-and-sections.md#type-parametrised-test-cases)
- [cmake-examples/unit_tests.cpp at master · ttroy50/cmake-examples · GitHub](https://github.com/ttroy50/cmake-examples/blob/master/05-unit-testing/catch2-vendored/unit_tests.cpp)


## CMake
- [GitHub - ttroy50/cmake-examples: Useful CMake Examples](https://github.com/ttroy50/cmake-examples)
- [unit testing - How to start working with GTest and CMake - Stack Overflow](https://stackoverflow.com/questions/8507723/how-to-start-working-with-gtest-and-cmake)

## Google Benchmark
昨天用benchmark去测试一个函数的性能，遇到一个问题，我怎么在测试完成后优雅的退出？benchmark的每个测试，就是一个for循环，把FUT执行n次。这里的n有一个初始值kMaxIterations = 1000000000，超级大。benchmark智能的点在于可以根据每次循环执行的时间自动调整实际循环的次数。但是如果我每次循环的时间补偿，并且没有支持循环这么多次的测试资源，我该怎么办？当然，我们可以当作一切都很正常，什么错误都没有发生。但是结果可能不对啊。如果按照文档里Exiting with an Error进行设置，这一项就输出一个错误信息，测试结果也拿不到。这么有名的一个测试框架，不可能不支持自定义循环次数啊。文档里没有，只能去代码里找。然后就把benchmark的代码快速过了一遍。先把BENCHMARK_MAIN()展开。

### ::benchmark::Initialize
解析、初始化参数。

### ::benchmark::RunSpecifiedBenchmarks()
- 创建打印报告类的实例default_display_reporter和default_file_reporter
- FindBenchmarksInternal()，这个函数是从families_中取出Benchmark对象，然后初始化BenchmarkInstance对象实例（这里有iterations的设置），然后实例move到benchmarks里。
- 然后调用internal::RunBenchmarks()。这就是从benchmarks取出实例，然后调用benchmark::internal::RunBenchmark()运行每一个BenchmarkInstance实例。而这个RunBenchmark()创建了一个BenchmarkRunner，在构造的时候传入BenchmarkInstance对象。什么？测试就是在这个构造函数里做的？没错，就是这里。DoOneRepetition()是主入口，DoNIterations()跟上，（这里搞了个ThreadPool貌似）然后调用RunInThread，然后调用BenchmarkInstance的Run函数，传入了循环次数IterationCount iters。如果BenchmarkInstance对象里的iterations为0（默认值，用户没有主动设置），iters设为1；否则，就是用户设置的iterations。这个iterations是从哪里来的呢？
- 还记得FindBenchmarks()里有一个families_，families_里有一个family，这个family是一个指向Benchmark对象的std::unique_ptr的引用。Benchmark有一个iterations_成员变量。也是在这里，Benchmark的iterations_赋值给了BenchmarkInstance的iterations。
- Benchmark类里有一个Iterations()方法，可以设置iterations_。
- 其实这里还有一个问题，这个families_中的family是怎么来的？families_是一个BenchmarkFamilies对象，用了单例模式。BenchmarkFamilies只提供了AddBenchmark()接口来增加Benchmark对象。只有RegisterBenchmarkInternal()调用了AddBenchmark()接口。那么哪里调用的RegisterBenchmarkInternal()呢？在benchmark.h的BENCHMARK×宏里。也是醉了。

### BENCHMARK_PRIVATE_CONCAT(a, b, c)
生成一个abc的字面量。

### BENCHMARK_PRIVATE_NAME(n)
```C++
BENCHMARK_PRIVATE_CONCAT(_benchmark_, BENCHMARK_PRIVATE_UNIQUE_ID, n)
// _benchmark_就是字面量_benchmark_。本来还以为是哪里定义的特殊符号，实际编译运行才发现，就是它本身所表达的意思。
// BENCHMARK_PRIVATE_UNIQUE_ID，就是__COUNTER__，每用一次就加1
```

### BENCHMARK_PRIVATE_DECLARE(n)
定义了一个static Benchmark指针。为啥是static？可能是为了在main()函数运行之前就完成初始化操作。

### BENCHMARK(n)
n是一个函数。new 一个FunctionBenchmark对象，并将作为参数输入RegisterBenchmarkInternal()，返回的指针赋值给static Benchmark指针。

### BENCHMARK_F(BaseClass, Method)
声明一个继承自BaseClass的类，类名为BaseClass_Method_Benchmark，类中声明一个虚函数BenchmarkCase()。然后new一个BaseClass_Method_Benchmark对象，传递给egisterBenchmarkInternal()，返回的指针赋值给static Benchmark指针。然后是定义虚函数BenchmarkCase()。这个虚函数会在class Fixture的Run()中调用。

## Google Test
测试用例或者测试夹具，如果是被测class的友元，他们需要在同一个命名空间里。否则获取私有变量失败。
- [Google C++单元测试框架GoogleTest---AdvancedGuide（译文）下 - 超超boy - 博客园](https://www.cnblogs.com/jycboy/p/AdvancedGuide2.html)

## 类模板的友元
- [C++ - 类模板(class template)友元(friend) 的 全部六种形式 及 代码 - Mystra - CSDN博客](https://blog.csdn.net/caroline_wendy/article/details/16916441)
主要普通的类，无法声明或定义模板成员变量。但是可以定义模板成员方法。

## temporary of non-literal type  in a constant expression
change the `constexpr` to `const`, then define the static member variable.
- [error: in-class initialization of static data member * of non-literal type - 在到处之间找我 - CSDN博客](https://blog.csdn.net/sinat_41104353/article/details/84799805)

## gcc
```bash
# show the detailed commands and execute
$ gcc -v ...
# show the detailed commands
$ gcc -###
```

The position and sequence of the link libraries are important, some errors are triggerred by this issue.
```bash
$ /usr/lib/gcc/x86_64-linux-gnu/7/collect2 -plugin /usr/lib/gcc/x86_64-linux-gnu/7/liblto_plugin.so "-plugin-opt=/usr/lib/gcc/x86_64-linux-gnu/7/lto-wrapper" "-plugin-opt=-fresolution=/tmp/ccoiLW6c.res" "-plugin-opt=-pass-through=-lgcc_s" "-plugin-opt=-pass-through=-lgcc" "-plugin-opt=-pass-through=-lc" "-plugin-opt=-pass-through=-lgcc_s" "-plugin-opt=-pass-through=-lgcc" "--sysroot=/" --build-id --eh-frame-hdr -m elf_x86_64 "--hash-style=gnu" --as-needed -dynamic-linker /lib64/ld-linux-x86-64.so.2 -pie -z now -z relro -o test /usr/lib/gcc/x86_64-linux-gnu/7/../../../x86_64-linux-gnu/Scrt1.o /usr/lib/gcc/x86_64-linux-gnu/7/../../../x86_64-linux-gnu/crti.o /usr/lib/gcc/x86_64-linux-gnu/7/crtbeginS.o -L/usr/lib/gcc/x86_64-linux-gnu/7 -L/usr/lib/gcc/x86_64-linux-gnu/7/../../../x86_64-linux-gnu -L/usr/lib/gcc/x86_64-linux-gnu/7/../../../../lib -L/lib/x86_64-linux-gnu -L/lib/../lib -L/usr/lib/x86_64-linux-gnu -L/usr/lib/../lib -L/usr/lib/gcc/x86_64-linux-gnu/7/../../.. -L/usr/local/lib /tmp/cch1njWV.o "-lstdc++" -lm -lgcc_s -lgcc -lc -lgcc_s -lgcc -lbenchmark /usr/lib/gcc/x86_64-linux-gnu/7/crtendS.o /usr/lib/gcc/x86_64-linux-gnu/7/../../../x86_64-linux-gnu/crtn.o /usr/local/lib/libbenchmark.a
```

### std::experimental::filesystem
在C++17中filesystem已经成为标准库，可以直接通过std::filesystem引用。如果GCC的版本不够高，可能要通过std::experimental::filesystem这个方式引用。
- [c++ - C++17 support Eclipse Neon - Stack Overflow](https://stackoverflow.com/questions/44044404/c17-support-eclipse-neon)
- 
### error: explicit specialization in non-namespace scope
无法在模板类或者类的内部做特化、偏特化，移到类的外面即可。
```C++
class VarData
{
public:
    template < typename T > bool IsTypeOf (int index) const
    {
        return IsTypeOf_f<T>::IsTypeOf(this, index);
    }
};

template <> bool VarData::IsTypeOf < int > (int index) const
{
    return false;
}

template <> bool VarData::IsTypeOf < double > (int index) const
{
    return false;
}
```
- [c++ - GCC error: explicit specialization in non-namespace scope - Stack Overflow](https://stackoverflow.com/questions/5777236/gcc-error-explicit-specialization-in-non-namespace-scope)
- [linux template error: explicit specialization in non-namespace scope - doubleface999的博客 - CSDN博客](https://blog.csdn.net/doubleface999/article/details/55798730)

## gdb
### examine
x /nfu addr
x examine
n a number denotes how many units you want to print
f format, x for hex, d for dec, u for unsigned dec, o for oct, t for binary, a for address in hex, c for char, f for float
u the unit size, b for byte, h for short, w for 4 bytes, g for 8 bytes.
- [GDB查看指定内存地址的内容——指令x - Haifeng的专栏 - CSDN博客](https://blog.csdn.net/haifeng_gu/article/details/73928545)

### handle signal
handle SIGUSR1 nostop
- [如何在GDB中忽略Signal信号处理 - artine的专栏 - CSDN博客](https://blog.csdn.net/artine/article/details/80008529)
- [gdb中忽略信号处理 - brucexu1978的专栏 - CSDN博客](https://blog.csdn.net/brucexu1978/article/details/7721321)

## Template
- [c++ - Implementation C++14 make_integer_sequence - Stack Overflow](https://stackoverflow.com/questions/17424477/implementation-c14-make-integer-sequence)

### C++ syntax for explicit specialization of a template function in a template class?
目前C++无法特化或者偏特化一个模板类中的模板方法。或者可能要先特化模板类，然后特化模板方法。也可以换一种方式来达到类似的目的：
```C++
template<class T, class Tag>
struct helper {
    static void f(T);   
};

template<class T>
struct helper<T, tag1> {
    static void f(T) {}
};

template<class T>
struct C {
    // ...
    template<class Tag>
    void foo(T t) {
        helper<T, Tag>::f(t);
    }
};
```
- [C++ syntax for explicit specialization of a template function in a template class? - Stack Overflow](https://stackoverflow.com/questions/2097811/c-syntax-for-explicit-specialization-of-a-template-function-in-a-template-clas)
- [c++ 模板类特化 - - ITeye博客](https://javaeye-hanlingbo.iteye.com/blog/2407800)
- [C++模板编程中只特化模板类的一个成员函数 - 莫水千流 - 博客园](https://www.cnblogs.com/zhoug2020/p/6581477.html)

## functional
### std::bind
bind传入的std::placeholders::_1，表示的是调用bind返回的函数对象时，传入的第一个参数。
取类的非静态成员函数地址，写法需要注意&CFuntion::ClassMember，也是奇葩。

## lambda
### C++0x lambda capture by value always const?
```C++
auto bar = [=] () mutable -> bool ...
// Without mutable you are declaring the operator () of the lambda object const.

// In some cases, we may need std::remove_const template.
```
- [c++ - C++0x lambda capture by value always const? - Stack Overflow](https://stackoverflow.com/questions/2835626/c0x-lambda-capture-by-value-always-const)
- [Lambda expressions (since C++11) - cppreference.com](https://en.cppreference.com/w/cpp/language/lambda)

## eclipse CDT
###　C++11
```text
Step 1：
Project->Properties->C/C++ Build->Settings->GCC G++ Compiler->Miscellaneous->Other flags 将-c -fmessage-length=0 改为 -c -fmessage-length=0 -std=c++11

Step 2：
C/C++ General -> Paths and Symbols -> Symbols -> GNU C++. 点击 "Add..." and 在Name 中添加__GXX_EXPERIMENTAL_CXX0X__  ，"Value" 不设值，点击apply

Step 3：
C/C++ General -> Paths and Symbols -> Symbols -> GNU C++. 点击 "Add..." ，Name 写 __cplusplus， value 写 201103L，点击 apply
```
- [让eclipse支持C++11特性 - 努力努力再努力！！！ - CSDN博客](https://blog.csdn.net/piaoliangjinjin/article/details/80701532)

## VNote
### Build from source
```bash
$ git clone https://github.com/tamlok/vnote.git
$ cd vnote
$ git submodule update --init
$ cd ..
# download QT SDK from https://mirrors4.tuna.tsinghua.edu.cn/qt/official_releases/qt/5.9/, then install it.
$ chmod +x qt*
$ git clone https://github.com/Kitware/CMake.git
$ cd CMake
$ ./bootstrap && make && sudo make install
$ cd ../vnote
$ mkdir build
$ cd build
$ cmake -DQt5_DIR=/home/liu/Qt5.9.5/5.9.5/gcc_64/lib/cmake/Qt5/ ..
$ cd src 
$ ./VNote
```
- [GitHub - Kitware/CMake: Mirror of CMake upstream repository](https://github.com/Kitware/CMake)
- [cmake - How to set the CMAKE_PREFIX_PATH? - Stack Overflow](https://stackoverflow.com/questions/8019505/how-to-set-the-cmake-prefix-path)
- [Viki - A simple Wiki page in Markdown from notebook of VNote](https://tamlok.github.io/vnote/zh_cn/#!docs/%E5%BC%80%E5%8F%91%E8%80%85/%E6%9E%84%E5%BB%BAVNote.md)
- [清华大学开源软件镜像站 \\| Tsinghua Open Source Mirror](https://mirrors4.tuna.tsinghua.edu.cn/qt/official_releases/qt/5.9/)
- [解决 QtCreator 3.5(4.0)无法输入中文的问题 - 乌合之众 - 博客园](https://www.cnblogs.com/oloroso/p/5114041.html)

# invalid use of non-static data member
nested class still need an instance of enclosing class, to get the member of enclosing class
``` C++
class foo {
  protected:
    int a;
  public:
    class bar {
      public:
        int getA() {return a;}   // ERROR
    };
    foo()
      : a (p->param)
};

    class bar {
      private:
        foo * const owner;
      public:
        bar(foo & owner) : owner(&owner) { }
        int getA() {return owner->a;}  // No ERROR
    };
```
- [c++ - invalid use of non-static data member - Stack Overflow](https://stackoverflow.com/questions/9590265/invalid-use-of-non-static-data-member)

# Specializing inner template with default parameters
That is not allowed. You cannot fully specialize a member of a class template that has not been itself fully specialized.
- [c++ - Specializing inner template with default parameters - Stack Overflow](https://stackoverflow.com/questions/17129543/specializing-inner-template-with-default-parameters)

# Create N-element constexpr array in C++11
```C++
#include <iostream>

template<int N>
struct A {
    constexpr A() : arr() {
        for (auto i = 0; i != N; ++i)
            arr[i] = i; 
    }
    int arr[N];
};

int main() {
    constexpr auto a = A<4>();
    for (auto x : a.arr)
        std::cout << x << '\\n';
}
```
```C++
#include <iostream>

template<int N, int... Rest>
struct Array_impl {
    static constexpr auto& value = Array_impl<N - 1, N, Rest...>::value;
};

template<int... Rest>
struct Array_impl<0, Rest...> {
    static constexpr int value[] = { 0, Rest... };
};

template<int... Rest>
constexpr int Array_impl<0, Rest...>::value[];

template<int N>
struct Array {
    static_assert(N >= 0, "N must be at least 0");

    static constexpr auto& value = Array_impl<N>::value;

    Array() = delete;
    Array(const Array&) = delete;
    Array(Array&&) = delete;
};

int main() {
    std::cout << Array<4>::value[3]; // prints 3
}
```
- [c++ - Create N-element constexpr array in C++11 - Stack Overflow](https://stackoverflow.com/questions/19019252/create-n-element-constexpr-array-in-c11)

# IO
## 禁用科学计数法
```C++
std::cout << fixed; // 禁用科学计数法
std::cout << setprecision(N); // 设置显示精度。cppreference的例子是含小数点共N位，但是在Linux上实际结果是小数点后共N位。可能跟具体实现有关系。
```
- [std::setprecision - cppreference.com](https://zh.cppreference.com/w/cpp/io/manip/setprecision)

## 设置宽度和填充
```C++
#include <iostream>
#include <iomanip>

std::stringstream ss
ss << "something" << setw(4) << setfill('0') << number << std::endl;
```
- [c++ int 转 string 实现前缀补0 - hellowOOOrld - 博客园](https://www.cnblogs.com/hellowooorld/p/7810849.html)

# pprof 使用指南
## 安装
```bash
$ git clone https://github.com/gperftools/gperftools.git
$ cd gperftool
$ ./autogen.sh
$ ./configure
$ make -j8
$ sudo make install
```
- [性能测试工具gperftools使用 - 流了个火 - 博客园](https://www.cnblogs.com/gnivor/p/11719958.html)

## 使用
```bash
$ g++ -O3 -L/usr/lib -L/usr/local/lib VectorTest.cpp -o test -lbenchmark -lpthread -lgtest -ltcmalloc
$ LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib HEAPPROFILE=/tmp/netheap ./test
$ pprof ./test /tmp/netheap.0001.heap --gv
$ pprof ./test /tmp/netheap.0001.heap --svg > netheap.svg
```
- [GitHub - gperftools/gperftools: Main gperftools repository](https://github.com/gperftools/gperftools)
- [pprof函数名未翻译、为函数地址0x00000232382788 - zhjh256 - 博客园](https://www.cnblogs.com/zhjh256/p/9470745.html)

# LEX and YACC

- [Yacc 与 Lex 快速入门](https://www.ibm.com/developerworks/cn/linux/sdk/lex/)
- [](https://blog.csdn.net/huyansoft/article/details/8860224)
- [Lex和Yacc -](https://blog.csdn.net/pandaxcl/category_188988.html)
- [hemny - 简书](https://www.jianshu.com/u/aaa5eddef78e)

