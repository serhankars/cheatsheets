# 1. \<string\> Operations

## Size Operations
    unsigned sz = str.size();
    str.resize(14);
    str.resize(sz+2,'+');

    while (!line.empty()){}
    str.clear();

## Element Access
    str.front();
    str.back();

## Modifiers

### append
    string& append(const string& str);
    string& append(const string& str, size_t subpos, size_t sublen)string& append(const char* s);
    string& append(const char* s, size_t n); //Appends a copy of the first n characters in the array of characters pointed by s.
    string& append(size_t n, char c);
    
    template <class InputIterator> 
    string& append(InputIterator first, InputIterator last);

  
 ### insert

> **insert() generally get pos(size_t type) as argument, but some versions take iterator too.**

    std::string str="to be question";
    std::string str2="the ";
    std::string str3="or not to be";
    std::string::iterator it;

    str.insert(6,str2); // to be (the )question
    str.insert(6,str3,3,4);  // to be (not )the question
    str.insert(10,"that is cool",8); // to be not (that is ) question
    str.insert(10,"to be "); // to be not (to be )that is the question
    str.insert(15,1,':'); // to be not to be(:) that is the question
    it = str.insert(str.begin()+5,','); // to be(,) not to be: that is the question
    str.insert(str.end(),3,'.'); // to be, not to be: that is the question(...)
    str.insert(it+2,str3.begin(),str3.begin()+3);

### erase
    str.erase(10,8); //pos, len
    str.erase(str.begin()+9);  
    str.erase(str.begin()+5, str.end()-9)

## String Operations
    const char* c_str() const;
    size_t find (const string& str, size_t pos = 0) const;
    size_t find (const char* s, size_t pos = 0) const;
    size_t find (const char* s, size_t pos, size_t n) const;
    size_t find (char c, size_t pos = 0) const;    

    std::size_t found = str.find(str2);
    if (found!=std::string::npos)
        std::cout << "first is found at: " << found << '\n'; 

    string substr (size_t pos = 0, size_t len = npos) const;        
*****
# 2. \<vector\> Operations

## Constructors
      std::vector<int> first; // empty vector of ints
      std::vector<int> second (4,100); // four ints with value 100
      std::vector<int> third (second.begin(),second.end()); // iterating through second
      std::vector<int> fourth (third); // a copy of third

## Size Operations
    myvector.size()
    myvector.resize(5);
    myvector.resize(size,default value);
    myvector.empty() // true or false 

## Element Access
    int a = myvector[1];
    myvector.front();
    myvector.back();
## Modifiers
    myvector.push_back(myint);
    myvector.pop_back();

    myvector.insert(it,200); // single element
    myvector.insert(it,2,300); // multiple elements 2 300s
    myvector.insert(it+2,anothervector.begin(),anothervector.end()); // range insertion

    // erase the 6th element
    myvector.erase (myvector.begin()+5);
    // erase the first 3 elements:
    myvector.erase (myvector.begin(),myvector.begin()+3);
    // to erase the last element
    m_data.erase(m_data.end()-1);

    myvector.clear();

# 2. \<list\> Operations
    std::list<int> mylist (2,100);  // two ints with a value of 100
    mylist.push_front (200);
    mylist.push_back (100);
    mylist.pop_back();
    mylist.insert (it,10);
    
    list<MyListNode*>::iterator it;
    for(it=m_nodes.begin();it!=m_nodes.end();it++)
    {
        if((*it)->m_key==key)
        {
            m_nodes.erase(it);
            return;
        }
    }
# 3. \<unordered_set\> Operations

> unordered_set containers are faster than set containers to access individual elements by their key, although they are generally less efficient for range iteration through a subset of their elements.

    std::unordered_set<std::string> myset = { "red","green","blue" };
    std::unordered_set<std::string>::const_iterator got = myset.find (input);

    if (myset.count(x)>0)
      std::cout << "myset has " << x << std::endl;
    else
      std::cout << "myset has no " << x << std::endl;
    
    myset.insert(mystring);
    myset.erase(myset.begin()); // erasing by iterator
    myset.erase ( "France" ); // erasing by key
    myset.erase ( myset.find("Japan"), myset.end() ); // erasing by range       

# 4. \<unordered_map\> Operations
    template<class Key,
             class T,
             class Hash = std::hash<Key>,
             class KeyEqual = std::equal_to<Key>,
             class Allocator = std::allocator<std::pair<const Key,T>>
            > class unordered_map;

        unordered_map<int,int> theMap;
        int numberOfNums = nums.size();

        for(int i=0;i<numberOfNums;i++)
        {
           if(theMap.find(target-nums[i])!= theMap.end())
               return {i,theMap[target-nums[i]]};
            else
            {
                theMap[nums[i]]=i;
            }
        }

        for (int i{1}; i != 8; ++i)
        {
          std::cout << "dict.count(" << i << ") = " << dict.count(i) << '\n';
        }

 # 4. Container Adapters
 
 ## 4.1 \<priority_queue\> Operations
> A priority queue is a container adaptor that provides constant time lookup of the <b>largest (by default)</b> element, at the expense of logarithmic insertion and extraction. 

    template<
           class T,
           class Container = std::vector<T>,
           class Compare = std::less<typename Container::value_type>
       > class priority_queue;

        std::priority_queue<Event> events;
        
        events.push(e);
        
        for (; !events.empty(); events.pop()) {
                Event const& e = events.top();
                std::cout << e << ' ';
        }
        
        auto comp = [&theMap](int n1,int n2){ return theMap[n1]> theMap[n2]; };
        // min queue
        priority_queue<int,vector<int>,decltype(comp)> heap(comp);

 ## 4.2 \<stack\> Operations
    size_t size = stack.size();
    if(container.empty()){}
    brackets_stack.push(pos);
    s.pop();
    std::cout << "Top element: " << s.top() << '\n';

 ## 4.3 \<queue\> Operations
    front()
    back()
    push()
    pop()
    if(m_queue.size()>m_capacity){}    

# Algorithms
## sort()
    std::sort(s.begin(), s.end(), std::greater<int>());
    print("sorted with the standard library compare function object");
    
    struct {
        bool operator()(int a, int b) const { return a < b; }
    } customLess;
    std::sort(s.begin(), s.end(), customLess);
    
    sort(vect.begin(), vect.end(),
    [](int a, int b) 
                   { 
                     return abs(a) > abs(b);
                    });
## Heap Operations
> The element with the highest value is always pointed by <b>first.</b> The order of the other elements depends on the particular implementation, but it is consistent throughout all heap-related functions of this header.

    int myints[] = {10,20,30,5,15};
    std::vector<int> v(myints,myints+5); 
    std::make_heap (v.begin(),v.end());
    std::pop_heap (v.begin(),v.end()); v.pop_back();
    v.push_back(99); std::push_heap (v.begin(),v.end());  

    // for min heap
    struct greater1{
      bool operator()(const long& a,const long& b) const{
        return a>b;
      }
    };

# Utilities
## hash
    #include <functional>
    std::size_t operator()(S const& s) const noexcept
    {
        std::size_t h1 = std::hash<std::string>{}(s.first_name);
        std::size_t h2 = std::hash<std::string>{}(s.last_name);
        return h1 ^ (h2 << 1); // or use boost::hash_combine
    }