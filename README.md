# Checklist every developer should follow
### For achieving error free and efficient code base every developer, programmer should follow these steps.

## 1. Null safety checking
**Null safety** is one of the great things to check before merging any code. **Null pointer** exceptions are very common in general development and it leads to compilation error even crashes. 

So whenever we take's value from user and parse from server API we should check for null or empty value before working with that data. If the the data is **NULL** or **EMPTY** we should handle that properly.

Suppose we are getting a value from args, we can check if it is null or not before working with the value,

``` 
args?.getString("ARG_ONE)?.let {
            smallGreenButtonView.setRightBtnText(it)
        }
``` 
or when parse a value from API we should check if it is null or not before using it,


```
if(value!=null || value!=""){
   //do task
}
``` 
## 2. Using Try/Catch
Using **Try/catch** when we are working with file, camera operations, UI element transactions such as Dialog, Fragments, etc. and in sensor, network operations is can be very important for some cases. As we know exception handling with try catch is very expensive but when we develop something we should avoid as much crashes as possible. Using try/catch can be the best last step to do to secure our user experience.  

Suppose we want to install ProviderInstaller but sometimes it throws error, which can lead to crashes. We can avoid this by using try/catch


```
     try {
           ProviderInstaller.installIfNeeded(this)
           } catch (e: GooglePlayServicesRepairableException) {
                e.printStackTrace()
           } catch (e: GooglePlayServicesNotAvailableException) {
                e.printStackTrace()
      }
``` 
## 3. Avoiding network operations in main thread

**Main thread or UI thread** is the thread which is used by the user interface and application when users are interacting with the application. So we should never make that laggy and slow. But whenever we consume and do complex network operations in main thread it slows down this thread. Thus it can make user experience little bit laggy but sometimes it can freeze the UI even crashes the UI in lower tier devices. We can simply avoid these by using **Coroutinescope IO** thread instead.


```
    CoroutineScope(Dispatchers.IO).launch {
            logThis("$tag scope launched")
            val list = appDatabase.agencyDao().getAllSelected()
            withContext(Dispatchers.Main) {
                logThis("$tag callback invoked on UI from launch scope")
                submitList(list)
            }
        }
``` 
Above code launched a IO scope then it handle the network and database operations. After completing the database operations it submitting the list in main thread with the help of **withContext(Dispatchers.MAIN)**.

## 4. Avoid duplicate code's

We should never use duplicate code in our application or development. Suppose we are needing a element in separate classes and activity. In this matter we should never copy paste that element in our code base but to create a** custom element or view **and then we can just implement in whenever we need to.
In android we also have **extension function**. It can be also used to reduce code duplication by creating a extension function which do a thing and that can be used in everywhere when we need. 

Now suppose we want to **toggle** a view to hide and show to the user when they press. Instead of coding VIEW/GONE for each view we can just create an extension function for the user. Then we can use it,

> Extension function

```

fun View.toggleView() {
    if (this.visibility == View.VISIBLE) {
        this.animate().duration = 200
        this.visibility = View.GONE
    } else {
        this.animate().duration = 200
        this.visibility = View.VISIBLE
    }
}
``` 
> Using it for the view

```
 textview.toggleView()
``` 
Here by just calling toggleView function we will be able to handle visibility and hiding action of the view.

## 5. Check memory leak 
Even the best developer can face this issue. But it can be easily avoided. Before finalizing or uploading any application we should find memory leaks in our application. Also when we write code we should take care of creating **Strong Reference**. We should use **Weak, Soft, and Phantom References** in order to make our application more flexible and memory leak free. 
For reference, [https://www.geeksforgeeks.org/types-references-java/](See this article for more understanding)

For android applications we can use [https://github.com/square/leakcanary](Leakcanary) which is a great tool for checking memory leak in our application. 

## 6. Using comment to make your code understandable 

Using comment's in owns code is a great practice. Whenever we write code we should write comments so that our code can be understand by others. Sometimes we also forget what we did after 4 5 years. So, it is best practice to mention what we have done, why we write this code and what does parameters do in comments. 

## 7. Commit small complete changes 

Always try to commit small changes which is complete by its functionality.  If we commit multiple functionality in one commit, then it becomes hard to find what is committed and where. By committing small complete changes it become's easy to find the issue, reverting back or removing, updating.  

## 8. Following standard meaningful naming convention 
Naming conventions are the key of understanding any variable, object, activity, and element of an application. If you do not follow proper meaningful naming conventions, it will become very hard to understand the code you have wrote and with the function what you really want to do. If meaningful naming needs big name's do it, but it should be meaningful.

Example, 

```
fun requestCameraPermission(){
   //your code
}

lateinit var btnCamera : Button 
``` 
Some short term to include in names,

```
Button - btn
EditText - et
TextView - tv
ProgressBar - pb
Checkbox - chk
RadioButton - rb
ToggleButton - tb
Spinner - spn
Menu - mnu
ListView - lv
GalleryView - gv
LinearLayout -ll
RelativeLayout - rl

and 

CLASS: <ClassName>
ACTIVITY: <ClassName>**Activity**
LAYOUT: classname_activity
COMPONENT IDS: classname_activity_component_name
``` 


