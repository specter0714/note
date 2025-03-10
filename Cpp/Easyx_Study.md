# 简单绘图

```c++
#include<graphics.h>

initgraph(int width, int height, int flag = NULL);//窗口
cleardevice();//清除窗口中的绘画
closegraph();//关闭窗口

circle(int x, int y, int r);//r是半径
fillcircle(int x, int y, int r);//有边框实心圆
solidcircle(int x, int y, int r);//无边框实心圆

setfillcolor(YELLOW);//填充黄色
setlinecolor(GREEN);//边框为绿色
setlinestyle(PS_SOLID, 5);//设置线条样式为实线，线条宽度为5

IMAGE image;//声明一个图片的变量
loadimage(IMAGE* pDstlmg, LPCTSTR plmgFile, int nWidth = 0, int nHeght = 0, bool nResize = false);//从文件中读取图像
//pDstlmg,保存图像的IMAGE对象的指针
//plmgFile,图片文件名
//nWidth = 0,图片的拉伸高度
//nHeight = 0,图片拉伸高度
//Bresize = false,是否调整IMAGE的大小以适应图片

putimage(int dstx, int dsty, IMAGE *pSrclmg, DWORD dwRop = SRCCOPY);//在当前设备上绘制指定图像
//dstX,绘制位置的x坐标
//dsty,绘制位置的y坐标
//pSrclmg,要绘制的IMAGE对象指针
//dwRop = SRCCOPY,三元光栅操作码


BOOL AlphaBlend(
       HDC hdcDest, //目标设备环境的句柄
       int nXOriginDest, //目标矩形左上角的x坐标
       int nYOriginDest, //目标矩形左下角的y坐标
       int nWidthDest, //目标矩形的宽度
       int nHeightDest, //目标矩形的高度
       HDC hdcSrc, //原设备环境句柄
       int nXOriginSrc, //源矩形左上角的x坐标
       int nYOriginSrc, //源矩形左上角的y坐标
       int nWidthSrc, //源矩形的宽度
       int nHeightSrc, //源矩形的高度
       BLENDFUNCTION blendFunction//一个结构类型参数，包含了源混合因子、目标混合因子、混合操作方式和透明度值等重要信息。
   );
//他的定义
typedef struct _BLENDFUNCTION {
       BYTE BlendOp;//混合操作的方式，AC_SRC_OVER表示源图像覆盖在目标图像上进行混合，这是最常用的混合方式之一
       BYTE BlendFlags;//保留供将来使用
       BYTE SourceConstantAlpha;//常量透明度值，范围从0（完全透明）到255.
       BYTE AlphaFormat;//指定源图像的透明度格式，例如，AC_SRC_ALPHA表示源图像包含自己的透明通道信息，会根据源图像自身的透明通道来进行混合。
   } BLENDFUNCTION, *PBLENDFUNCTION;



```

# 双缓冲技术

```c++
#include <graphics.h>
#include <stdio.h>

int main()
{
    initgraph(640, 480);

    // 创建一个与绘图窗口大小相同的内存设备环境
    HDC memDC = CreateCompatibleDC(NULL);
    // 创建一个与绘图窗口大小相同的位图
    HBITMAP memBitmap = CreateCompatibleBitmap(GetImageHDC(), 640, 480);
    // 选择位图到内存设备环境
    HBITMAP oldBitmap = (HBITMAP)SelectObject(memDC, memBitmap);

    // 在内存设备环境上进行绘图
    setfillcolor(WHITE);
    fillrectangle(0, 0, 640, 480);
    settextcolor(BLACK);
    outtextxy(200, 200, "Using Double Buffering");

    // 将内存设备环境中的内容复制到屏幕设备环境
    BitBlt(GetImageHDC(), 0, 0, 640, 480, memDC, 0, 0, SRCCOPY);

    // 释放资源
    SelectObject(memDC, oldBitmap);
    DeleteObject(memBitmap);
    DeleteDC(memDC);

    getchar();
    closegraph();
    return 0;
}
```

# 控制帧率

```c++
#include <graphics.h>
#include <stdio.h>
#include <windows.h>

const int FPS = 60;
const int DELAY_TIME = 1000 / FPS;

int main()
{
    initgraph(640, 480);

    while (true)
    {
        // 记录开始时间
        DWORD start = GetTickCount();

        // 绘图代码
        setfillcolor(WHITE);
        fillrectangle(0, 0, 640, 480);
        settextcolor(BLACK);
        outtextxy(200, 200, "Controlling FPS");

        // 计算并控制帧率
        DWORD elapsed = GetTickCount() - start;
        if (elapsed < DELAY_TIME)
        {
            Sleep(DELAY_TIME - elapsed);
        }

        if (GetAsyncKeyState(VK_ESCAPE))
        {
            break;
        }
    }

    closegraph();
    return 0;
```

# 多线程（使用c++标准库\<thread>）

```c++
#include <graphics.h>
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
IMAGE img;
bool imgLoaded = false;

// 线程函数，用于加载图片
void loadImage(const wchar_t* path) {
    if (!loadimage(&img, path)) {
        std::cerr << "Failed to load image" << std::endl;
    }
    {
        std::lock_guard<std::mutex> lock(mtx);
        imgLoaded = true;
    }
}

int main() {
    initgraph(800, 600);

    std::thread loadingThread(loadImage, L"image.jpg");

    // 主线程继续执行其他任务
    while (!imgLoaded) {
        // 可以在这里绘制一些默认画面或者处理其他逻辑
        setfillcolor(WHITE);
        fillrectangle(0, 0, 800, 600);
        settextcolor(BLACK);
        outtextxy(300, 300, "Loading image...");
        Sleep(10);
    }

    {
        std::lock_guard<std::mutex> lock(mtx);
        putimage(0, 0, &img);
    }

    loadingThread.join();
    Sleep(3000);
    closegraph();
    return 0;
}
```

# 多线程（使用Windows API）

```c++
#include <graphics.h>
#include <windows.h>
#include <iostream>

HANDLE hThread;
DWORD threadID;
IMAGE img;
bool imgLoaded = false;
CRITICAL_SECTION cs;

// 线程函数，用于加载图片
DWORD WINAPI LoadImageThread(LPVOID lpParam) {
    const wchar_t* path = static_cast<const wchar_t*>(lpParam);
    if (!loadimage(&img, path)) {
        std::cerr << "Failed to load image" << std::endl;
    }
    EnterCriticalSection(&cs);
    imgLoaded = true;
    LeaveCriticalSection(&cs);
    return 0;
}

int main() {
    initgraph(800, 600);

    // 初始化临界区
    InitializeCriticalSection(&cs);

    hThread = CreateThread(NULL, 0, LoadImageThread, L"image.jpg", 0, &threadID);
    if (hThread == NULL) {
        std::cerr << "Failed to create thread" << std::endl;
        return 1;
    }

    // 主线程继续执行其他任务
    while (!imgLoaded) {
        // 可以在这里绘制一些默认画面或者处理其他逻辑
        setfillcolor(WHITE);
        fillrectangle(0, 0, 800, 600);
        settextcolor(BLACK);
        outtextxy(300, 300, "Loading image...");
        Sleep(10);
    }

    putimage(0, 0, &img);

    // 等待线程结束
    WaitForSingleObject(hThread, INFINITE);
    CloseHandle(hThread);

    // 删除临界区
    DeleteCriticalSection(&cs);

    Sleep(3000);
    closegraph();
    return 0;
}
```

# 批量绘图函数（双缓存）

```c++
beginbatchdraw();//用于开启批量绘图的函数
    
    

while(){
    //绘图
    
    flushbatchdraw();//绘图函数绘图后不会马上显示出来，只会绘制在缓冲区，是哟个flushbatchdraw函数后才会一起显现，减少画面刷新次数，避免画面闪烁，
}
```

