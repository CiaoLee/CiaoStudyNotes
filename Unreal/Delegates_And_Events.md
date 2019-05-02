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