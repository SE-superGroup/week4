// experiment1.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include "./gdal/gdal_priv.h"
#pragma comment(lib, "gdal_i.lib")

using namespace std;


int _tmain(int argc, _TCHAR* argv[])
{
​	void deal_1();
​	void deal_2();
​	void deal_3();
​	void deal_4();
​	void deal_5();
​	void deal_6();
​	deal_1();
​	deal_2();
​	deal_3();
​	deal_4();
​	deal_5();
​	deal_6();

	return 0;
}

void deal_1(){
​	//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena1.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);

   for(t=1;t<4;t++){
​		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
​		for(i=1;i<=imgYlen-1;i++){
​			for(j=1;j<=imgXlen-1;j++){
​				dstBuff[i*imgXlen+j]=(float)((srcBuff[(i-1)*imgXlen+j]+srcBuff[(i+1)*imgXlen+j]+srcBuff[i*imgXlen+j-1]+srcBuff[i*imgXlen+j+1]+srcBuff[i*imgXlen+j])*0.2);//上下左右自己*0.2
​			}
​		}
​		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
​	}

   		//处理边缘像素
​	for(j=0;j<imgXlen;j++)//第一行像素值为0
​		dstBuff[j]=(float)0;
​	for(t=1;t<4;t++)
​		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);

	for(j=(imgXlen-1)*imgYlen;j<(imgXlen*imgYlen);j++)//最后一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=0;j<imgXlen;j++)//第一列像素值为0
		dstBuff[j*imgXlen]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=1;j<imgXlen;j++)//最后一列像素值为0
		dstBuff[j*imgXlen-1]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");
}

void deal_2(){
​	//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena2.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	for(t=1;t<4;t++){
		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
		for(i=2;i<imgYlen-2;i++){
			for(j=2;j<imgXlen-2;j++){
				dstBuff[i*imgXlen+j]=(GByte)((srcBuff[(i-2)*imgXlen+j-2]+srcBuff[(i-1)*imgXlen+j-1]+srcBuff[i*imgXlen+j]+srcBuff[(i+1)*imgXlen+j+1]+srcBuff[(i+2)*imgXlen+j+2])*0.2);
			}
		}
		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
	}
	
			//处理边缘像素
	for(j=0;j<imgXlen;j++)//第一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=(imgXlen-1)*imgYlen;j<(imgXlen*imgYlen);j++)//最后一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=0;j<imgXlen;j++)//第一列像素值为0
		dstBuff[j*imgXlen]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=1;j<imgXlen;j++)//最后一列像素值为0
		dstBuff[j*imgXlen-1]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");

}

void deal_3(){
​	//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena3.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	for(t=1;t<4;t++){
		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
		for(i=1;i<=imgYlen-1;i++){
			for(j=1;j<=imgXlen-1;j++){
				dstBuff[i*imgXlen+j]=(float)(
					-srcBuff[(i-1)*imgXlen+j-1]-srcBuff[(i-1)*imgXlen+j]-srcBuff[(i-1)*imgXlen+j+1]
				    -srcBuff[i*imgXlen+j-1] + 8.0*srcBuff[i*imgXlen+j]-srcBuff[i*imgXlen+j+1]
					-srcBuff[(i+1)*imgXlen+j-1]-srcBuff[(i+1)*imgXlen+j]-srcBuff[(i+1)*imgXlen+j+1]);//上下左右-1自己8
			}
		}
		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
	}
	
		//处理边缘像素
	for(j=0;j<imgXlen;j++)//第一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=(imgXlen-1)*imgYlen;j<(imgXlen*imgYlen);j++)//最后一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=0;j<imgXlen;j++)//第一列像素值为0
		dstBuff[j*imgXlen]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=1;j<imgXlen;j++)//最后一列像素值为0
		dstBuff[j*imgXlen-1]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");	
}

void deal_4(){
​	//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena4.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	for(t=1;t<4;t++){
		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
		for(i=1;i<=imgYlen-1;i++){
			for(j=1;j<=imgXlen-1;j++){
				dstBuff[i*imgXlen+j]=(float)(
					-srcBuff[(i-1)*imgXlen+j-1]-srcBuff[(i-1)*imgXlen+j]-srcBuff[(i-1)*imgXlen+j+1]
				    -srcBuff[i*imgXlen+j-1] + 9.0*srcBuff[i*imgXlen+j]-srcBuff[i*imgXlen+j+1]
					-srcBuff[(i+1)*imgXlen+j-1]-srcBuff[(i+1)*imgXlen+j]-srcBuff[(i+1)*imgXlen+j+1]);//上下左右-1自己9
			}
		}
		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
	}
	
		//处理边缘像素
	for(j=0;j<imgXlen;j++)//第一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=(imgXlen-1)*imgYlen;j<(imgXlen*imgYlen);j++)//最后一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=0;j<imgXlen;j++)//第一列像素值为0
		dstBuff[j*imgXlen]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=1;j<imgXlen;j++)//最后一列像素值为0
		dstBuff[j*imgXlen-1]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");	
}

void deal_5(){
​		//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena5.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	for(t=1;t<4;t++){
		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
		for(i=1;i<=imgYlen-1;i++){
			for(j=1;j<=imgXlen-1;j++){
				dstBuff[i*imgXlen+j]=(float)(
					-srcBuff[(i-1)*imgXlen+j-1]-srcBuff[(i-1)*imgXlen+j]
				    -srcBuff[i*imgXlen+j-1]+srcBuff[i*imgXlen+j+1]
					+srcBuff[(i+1)*imgXlen+j]+srcBuff[(i+1)*imgXlen+j+1]);
			}
		}
		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
	}
	
		//处理边缘像素
	for(j=0;j<imgXlen;j++)//第一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=(imgXlen-1)*imgYlen;j<(imgXlen*imgYlen);j++)//最后一行像素值为0
		dstBuff[j]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=0;j<imgXlen;j++)//第一列像素值为0
		dstBuff[j*imgXlen]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	for(j=1;j<imgXlen;j++)//最后一列像素值为0
		dstBuff[j*imgXlen-1]=(float)0;
	for(t=1;t<4;t++)
		poDstDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Byte,0,0);
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");
}

void deal_6(){
​	//输入图像
​	GDALDataset* poSrcDS;
​	//输出图像
​	GDALDataset* poDstDS;
​	//图像的宽度和高度
​	int imgXlen,imgYlen;
​	//输入图像路径
​	char* srcPath="lena.jpg";
​	//输出图像路径
​	char* dstPath="lena6.tif";
​	//图像内存存储
​	float* srcBuff;
​	float* dstBuff;
​	//图像波段数
​	int i,bandNum;

	//注册驱动
	GDALAllRegister();
	
	//打开图像
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	//获取图像宽度，高度和波段数量
	imgXlen=poSrcDS->GetRasterXSize();
	imgYlen=poSrcDS->GetRasterYSize();
	bandNum=poSrcDS->GetRasterCount();
	//输出获取的结果
	cout<<"Image X length: "<<imgXlen<<endl;
	cout<<"Image Y length: "<<imgYlen<<endl;
	//根据图像的宽度和高度分配内存
	srcBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	dstBuff=(float*)CPLMalloc(imgXlen*imgYlen*sizeof(float));
	//创建输出图像
	poDstDS=GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath,imgXlen,imgYlen,bandNum,GDT_Byte,NULL);
	int t=1,j;
	
	poSrcDS=(GDALDataset*)GDALOpenShared(srcPath,GA_ReadOnly);
	
	for(t=1;t<4;t++){
		poSrcDS->GetRasterBand(t)->RasterIO(GF_Read,0,0,imgXlen,imgYlen,srcBuff,imgXlen,imgYlen,GDT_Float32,0,0);
		for(i=2;i<imgYlen-2;i++){
			for(j=2;j<imgXlen-2;j++){
				dstBuff[i*imgXlen+j]=(float)((srcBuff[(i-2)*imgXlen+j-2]*0.0120+srcBuff[(i-2)*imgXlen+j-1]*0.1253+srcBuff[(i-2)*imgXlen+j]*0.2736+srcBuff[(i-2)*imgXlen+j+1]*0.1253+srcBuff[(i-2)*imgXlen+j+2]*0.0120
				+srcBuff[(i-1)*imgXlen+j-2]*0.1253+srcBuff[(i-1)*imgXlen+j-1]*1.3054+srcBuff[(i-1)*imgXlen+j]*2.8514+srcBuff[(i-1)*imgXlen+j+1]*1.3054+srcBuff[(i-1)*imgXlen+j+2]*0.1253
				+srcBuff[i*imgXlen+j-2]*0.2736+srcBuff[i*imgXlen+j-1]*2.8514+srcBuff[i*imgXlen+j]*6.2279+srcBuff[i*imgXlen+j+1]*2.8514+srcBuff[i*imgXlen+j+2]*0.2736
				+srcBuff[(i+1)*imgXlen+j-2]*0.1253+srcBuff[(i+1)*imgXlen+j-1]*1.3054+srcBuff[(i+1)*imgXlen+j]*2.8514+srcBuff[(i+1)*imgXlen+j+1]*1.3054+srcBuff[(i+1)*imgXlen+j+2]*0.1253
				+srcBuff[(i+2)*imgXlen+j-2]*0.0120+srcBuff[(i+2)*imgXlen+j-1]*0.1253+srcBuff[(i+2)*imgXlen+j]*0.2736+srcBuff[(i+2)*imgXlen+j+1]*0.1253+srcBuff[(i+2)*imgXlen+j+2]*0.0120
				)/25.0);
			}
		}
		poDstDS->GetRasterBand(t)->RasterIO(GF_Write,0,0,imgXlen,imgYlen,dstBuff,imgXlen,imgYlen,GDT_Float32,0,0);
	}
	
	//清除内存
	CPLFree(srcBuff);
	CPLFree(dstBuff);
	//关闭dataset
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	
	system("PAUSE");

}