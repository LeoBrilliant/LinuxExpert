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
