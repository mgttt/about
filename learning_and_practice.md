personal tech learn/pratise log since 2019

### 2019-03

* 285mb size win7 [vdi image](https://github.com/wanjochan/PartnerNET.Software/blob/master/win7_000_d.7z?raw=true) for automation dev;

### 2019-02

* learn VirtualBox VDRP mode with TLS cert, cool for trading union usage;
* review dynamic-planning;
* new simple strategy for 725 trading code;

* review android [jni example](https://github.com/googlesamples/android-ndk/blob/master/hello-jniCallback/app/src/main/cpp/hello-jnicallback.c) to see if any solution to utilize the webview and v8 by linking with libwebcore.so
```
#https://stackoverflow.com/questions/6880778/android-utilize-v8-without-webview/11973689#11973689
#https://android.googlesource.com/platform/external/webkit
v8::Persistent<v8::Context> context = v8::Persistent<v8::Context>::New(v8::Context::New());
context->Enter();
```
```
WebCoreFrameBridge
/external/webkit/Source/WebKit/android/jni/WebCoreFrameBridge.cpp

static void AddJavascriptInterface(JNIEnv *env, jobject obj, jint nativeFramePointer,
    jobject javascriptObj, jstring interfaceName, jboolean requireAnnotation)
{
WebCore::Frame* pFrame = 0;
if (nativeFramePointer == 0)
    pFrame = GET_NATIVE_FRAME(env, obj);
else
    pFrame = (WebCore::Frame*)nativeFramePointer;
ALOG_ASSERT(pFrame, "nativeAddJavascriptInterface must take a valid frame pointer!");

JavaVM* vm;
env->GetJavaVM(&vm);
ALOGV("::WebCore:: addJSInterface: %p", pFrame);

if (pFrame) {
    RefPtr<JavaInstance> addedObject = WeakJavaInstance::create(javascriptObj,
            requireAnnotation);
    const char* name = getCharactersFromJStringInEnv(env, interfaceName);
    // Pass ownership of the added object to bindToWindowObject.
    NPObject* npObject = JavaInstanceToNPObject(addedObject.get());
    pFrame->script()->bindToWindowObject(pFrame, name, npObject);
    // bindToWindowObject calls NPN_RetainObject on the
    // returned one (see createV8ObjectForNPObject in V8NPObject.cpp).
    // bindToWindowObject also increases obj's ref count and decreases
    // the ref count when the object is not reachable from JavaScript
    // side. Code here must release the reference count increased by
    // bindToWindowObject.

    // Note that while this function is declared in WebCore/bridge/npruntime.h, for V8 builds
    // we use WebCore/bindings/v8/npruntime.cpp (rather than
    // WebCore/bridge/npruntime.cpp), so the function is implemented there.
    // TODO: Combine the two versions of these NPAPI files.
    NPN_ReleaseObject(npObject);
    releaseCharactersForJString(interfaceName, name);
}
}
```

* place holder for the [VM research](https://docs.google.com/document/d/1udOtqnWSzmhLBCRVEOQCUs_Kj3b81Y_YIDZW8laNbz8/edit)
* review [mathjs](http://mathjs.org/download.html), [numbers.js](https://github.com/numbers/numbers.js)

#### Official IB api

* https://www.interactivebrokers.com.au/en/index.php?f=5041
* http://interactivebrokers.github.io/#

#### TWSTOOL for Interactive Broker

* git clone https://github.com/rudimeier/twsapi.git, autoreconf -vfi, ./configure, make, sudo make install
* git clone https://github.com/rudimeier/twstools.git, autoreconf -vfi, ./configure, make, sudo make install
BUILD Failed, TBC

#### FUTU (C)
* install [google protobuf > 3.5.1](https://github.com/protocolbuffers/protobuf/releases), wget https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protobuf-cpp-3.6.1.tar.gz, tar xzvf, ./configure, make, sudo make install
* install [libuv >1.22], git clone https://github.com/libuv/libuv.git, make && make install
* install FUTU-C, git clone https://github.com/FutunnOpen/C-For-FutuOpenD.git , edit CMakeFile.txt (libuv_a.a to libuv.a) mkdir build && cd build && cmake .. && make
NOT PASSED YET, need more time;

* learn and compare OPCODE of python/lua/jerrscript VM
* quick review NectarJS (https://github.com/NectarJS/)
* improve EBIZ Control Center with Vue.js template syntax
* release [tinyfsm](https://github.com/wanjochan/tinyfsm)
* upgrade 20+ servers from win to lnx and tuned control center of EBIZ Union;
* learn [AWS Lambda Cron/Rate mode](https://docs.aws.amazon.com/lambda/latest/dg/tutorial-scheduled-events-schedule-expressions.html); with Lambda functions to build a robust quick tools-alike apps.
* learn [TencentCloud/ServlessFunction](https://console.cloud.tencent.com/scf/list), similiar to the AWS Lambda Functions, and much better to use....adopt it as our cronjob trigger perfect
* Learn and practise [CloudFlare Argo Tunnel](https://www.cloudflare.com/en-au/products/argo-tunnel/)
* Optimized [Normalization Distribution CDF](https://github.com/wanjochan/mini_js_warehouse/blob/master/BlackScholesMerton.js) and post to the https://www.johndcook.com/blog/cpp_phi/
* Improved FX trading algorithm "bowl-shake-water-base" trail 6K USD planned 3 months
* pack sign key and prepare pack-release-package scripts for uChatLite