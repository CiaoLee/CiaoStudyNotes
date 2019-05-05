# **Delegates And Events**

## **Delegates**

### **Declare And Definition**

- In **DelegateCombinations.h**
```c++
    //Delegates normal.
    #define DECLARE_DELEGATE( DelegateName ) FUNC_DECLARE_DELEGATE( DelegateName, void)

    //Multicast Delegate:
    #define DELCARE_MULTICAST_DELEGATE（ DelegateName ) FUNC_DECLARE_MULTICAST_DELEGATE( DelegateName, void)

    //Dynamic Delegate:
    #define DECLARE_DYNAMIC_DELEGATE( DelegateName ) BODY_MACRO_COMBINE(CURRENT_FILE_ID,_,__LINE__,_DELEGATE) FUNC_DECLARE_DYNAMIC_DELEGATE( FWeakObjectPtr, DelegateName, DelegateName##_DelegateWrapper, ,FUNC_CONCAT( * this ), void)
```

- In **Delegate.h**
```c++
    //Declare the user's delegate object
    //NOTE: The last parameter is variadic and is used as
    //the 'template args' for this delegate's classes(__VA_ARGS__)
    #define FUNC_DECLARE_DELEGATE( DelegateName,... )\
        typedef TBaseDelegate<__VA_ARGS__> DelegateName;

    //Declare the user's multicast delegate object.
    //NOTE: The last parametera is variadic and is used as
    //the 'template args' for this delegate's classes(__VA_ARGS__)
    #define FUNC_DECLARE_MULTICAST_DELEGATE(MulticastDelegateName,...)\
        typedef TMulticastDelegate<__VA_ARGS__> MulticastDelegateName;
```
- In **DelegateSignatureImpl.inl**

```C++
template<typename WrappedRetValType, typename... ParamType>
class TBaseDelegate: public FDelegateBase
{
    //Type definition for return value type
    //typename is necessary cause the 
    //TUnwrapType<WrappedRetValType>::Type compiler cannt
    //sure it's a type.
    typdef typename TUnwrapType<WrappedRetValType>::Type RetValType;

    typedef RetValType TFuncType(ParamTypes...);

    //Type definition for the shared interface of delegate
    //instance types compile with this delegate type.
    typedef IBaseDelegateInstance<TFuncType> TDelegateInstanceInterface;

    //Decalre the usre's "fast" shared pointer-based delegate isntance types.;
};
```

- In **DelegateInstanceInterface.h**

```c++
//Class representing an handle to a delegate.
//Consist by:
//
class FDelegateHandle
{

public:
    enum EGenerateNewHandleType
    {
        GenerateNewHandle
    };

    FDelegateHandle() : ID(0)
    {

    }

    explicit FDelegateHandle(EGenerateNewHandleType) : ID(GenerateNewID())
    {
    }

    bool IsValid() const
    {
        return ID != 0;
    }

    void Reset()
    {
        ID = 0;
    }
private:
    friend bool operator==(const FDelegateHandle& Lhs, const FDelegateHandle&I Rhs)
    {
        return Lhs.ID = Rhs.Id;
    }

    firend bool operator!=(const FDelegateHandle& Lhs, const FDelegateHandle&I Rhs)
    {
        return Lhs.ID != Rhs.ID;
    }
    
    friend FORCEINLINE uint32 GetTypeHash(const FDeleagetHandle& Key)
    {
        return GetTypeHash(Key.ID);
    }

    //Generates a new ID for luse the delegate handle

    static CORE_API unint64 GenerateNewId();

    uint64 ID;
}; 
//DelegateHandle.CPP
namespace UE4Delegates_Private
{
    TAtomic<uint64> GNextID(1);
}

uint6u4 FDelegateHandle::GenerateNewID()
{
    uint64 Result = ++ UE4Delegates_Private::GNextID;
    //Check for the next-to-impossible event that we
    //wrap around to 0, because we reserve 0 for null
    //delegates
    if(Result ==0)
    {
        Result = ++ UE4Delegates_Private::GNextID;
    }

    return Result;
}
//Interface for delegate instances;
class IDelegateInstance
{
public:
#if USE_DELEGATE_TRYGETBOUNDFUNCTIONNAME
//Tires return the name of a bound function.
//Returns NAME_None if the delegate is unbound or 
//a binding name is unavailable.

virtual FNAME TryGetrBoundFunctionName() const = 0;
#endif

//returns the UObject that is delgate instance is bound to.
virtual UOBject* GetUObject() const = 0;

//returns a pointer to an object bound to this delegate 
virtual UObject* GetUobject() const = 0;

//returns a pointer to an object bound to this delegate instance, intended for quick look up in the timer manager.

virtual const void* GetObjectForTimerManager() const = 0;

//returns true if this delegate is bound to the specified UserObject.
virtual bool HasSameObject(const void* InUserObject) const = 0;

//Check to see if the user object bound to this delegate can ever be valid used to compact multicast delegate arrays so they don't expand without limit.

virtual bool IsCompactable() const
{
    return !IsSafeToExecute();
}

//Checks to see if the user object bound toi this delegate is still valid

virtual bool IsSafeToExecute() const = 0;

//Returns a handle for the dlegate

virtual FDelegateHandle GetHandle() const = 0;

public:

//Virtual destructor.
virtual ~IDelegateInstance() {}
};
```

- In **DelegateBase.h**

```c++
//In TypeCompatibleBytes.h

//Base class for unicast delegates.
```

- In **TypeWrapper.h**

```C++
    //in TypeWrapper.h
    //Wraps a type
    /**
    * The inteded use of this template is to allow types to * passed around where the unwrapped type would  give different behavior.
    * An example off this is when you want a template specialization to refer to the 
    * infinite recursion through specialization, e.g.;
    */
    // Before
    template <typename T>
    struct TThing
    {
        void f(T t)
        {
            DoSomething(t);
        }
    };

    template<>
    struct TThing<int>
    {
        void f(int t)
        {
            DoSometingElseFirst(t);
            TThing<int>::f(t); //Infinite recursion.
        }
    }

    //after
    template <typename T>
    struct TThing
    {
        typedef typename TUnwrapType<T>::Type RealT
        void f(RealT t)
        {
            DoSomething(t);
        }
    }
    
    template <>
    struct TThing<int>
    {
        void f(int t)
        {
            DoSomethingElseFirst(t);
            TThing<TTypeWrapper<int>>::f(t);
        }
    }

    template <typename T>
    struct TTypeWrapper;

    //If not a TType Wrapper
    template <typename T>
    struct TUnwrapType
    {
        typedef T Type;
    }

    //Unwrap helper template class to the the True Type.
    template<typename T>
    struct TUnwrapType<TTypeWrapper<T>>
    {
        typedef T Type;
    }
```
