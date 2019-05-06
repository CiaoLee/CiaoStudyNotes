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
- In **DelegateBase**

```c++
//Base class for unicast delegates.
class FDelegateBase
{
    friend class FMulticastDelegateBase<FWeakObjectPtr>;

public:

    //Creates and initializes a new instance

    /**
    * 
    */
    explicit FDelegateBase():DelegateSize(0)
    {
        
    }

    ~FDelegateBase()
    {
        Unbind();
    }

    //Move constructor;
    FDelegateBase(FDelegateBase&& other)
    {
        DelegateAllocator.MoveToEmpty(Other.DelegateAllocator);
        DelegateSize = Other.DelegateSize;
        Other.DelegateSize = 0;
    }

    FDelegateBase& operator=(FDelegateBase&& Other)
    {
        Unbind();
        FDelegateAllocator.MoveToEmpty(Other.DelegateAllocator);
        DelegateSize = Other.DelegateSize;
        Other.DelegateSize = 0;
        return *this.
    }

    #if USE_DELEGATE_TRYGETBOUNDFUNCTIONNAME
    //Tries to return the name of a bound function. Return NAME_NONE if the delegate is unbound or a binding name is unavailable.

    FName TryGetBoundFunctionName() const
    {
        if(IDelegateInstance* Ptr = GetDelegateInstanceProtected())
        {
            return Ptr->TryGetBoundFunctionName();
        }
        return NAME_None;
    }

    FORCEINLINE class UObject* GetUObject() const
    {
        if(IDelegateInstance* Ptr = GetDelegateInstanceProtected())
        {
            return Ptr->GetUObject();
        }

        return nullptr;
    }
    #endif

    //Checks to see if the user object bound to this delgate is sill invalid.

    //return true if the user object is still valid and it's safe to execute the function call.

    FORCEINLINE bool IsBound() const
    {
        IDelegateInstance* Ptr = GetDelegateInstanceProtected();

        return Ptr&&Ptr->IsSafeToExecute();
    }

    //Returns a pointer to an object bound to this delegate
    FORCEINLINE const void* GetObjectForTimerManager() const
    {
        IDelegateInstance* Ptr = GetDelegateInstanceProtected();

        const void* Result = Ptr? Ptr->GetObjectForTimerMessage():nullptr;
        return Result;
    }

    //Checks to see if this delegate is bound to the given user object.
    FORCEINLINE bool IsBoundToObject(void const* InuserObject) const
    {
        if(!InUserObject)
        {
            return false;
        }

        IDelegateInstance* Ptr = GetDelegateInstanceProtected();
        return Ptr && Ptr->HasSameObject(InUserObject);
    }

    //Unbinds this delegate
    FORCEINLINE void Unbind()
    {
        if(IDelegateInstance* Ptr = GetDelegateInstanceProtected())
        {
            Ptr->~IDelegateInstance();
        }
    }
}
```
