# daily

1、
python通过clang来解析、编译C++，相关资料：
https://blog.csdn.net/snsn1984/article/details/25963601
https://blog.csdn.net/wangtua/article/details/77426883
2、
message = {"result":{"capabilities":{"completionProvider":{"triggerCharacters":[]}}}}
mesage = {"params":{"configurations":["browe":{"databaseFilename":""}]}

3、python+clang
#!/usr/bin/env python
""" Usage: call with <filename> <typename>
"""

import sys
import clang.cindex

def find_typerefs(node, typename):
    """ Find all references to the type named 'typename'
    """
    if node.kind.is_reference():
        ref_node = clang.cindex.Cursor_ref(node)
        if ref_node.spelling == typename:
            print 'Found %s [line=%s, col=%s]' % (
                typename, node.location.line, node.location.column)
    # Recurse for children of this node
    for c in node.get_children():
        find_typerefs(c, typename)

index = clang.cindex.Index.create()
tu = index.parse(sys.argv[1])
print 'Translation unit:', tu.spelling
find_typerefs(tu.cursor, sys.argv[2])

4、待解析C++
class Person {
};


class Room {
public:
    void add_person(Person person)
    {
        // do stuff
    }

private:
    Person* people_in_room;
};


template <class T, int N>
class Bag<T, N> {
};


int main()
{
    Person* p = new Person();
    Bag<Person, 42> bagofpersons;

    return 0;
}

5、开始解析【第一个class Person没有在里面？】
Translation unit: simple_demo_src.cpp
Found Person [line=7, col=21]
Found Person [line=13, col=5]
Found Person [line=24, col=5]
Found Person [line=24, col=21]
Found Person [line=25, col=9]

6、subprocess的学习
http://xstarcd.github.io/wiki/Python/python_subprocess_study.html
