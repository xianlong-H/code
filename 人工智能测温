#include <MPython.h>
#include <DFRobot_Iot.h>
#include <DFRobot_MLX90614.h>
#include <mPython_tinywebdb.h>
#include <DFRobot_HuskyLens.h>

// 动态变量
String mind_s_XingMing;
// 静态常量
const String topics[5] = {"6ablb9OGR","","","",""};
// 创建对象
DFRobot_Iot       myIot;
mPython_TinyWebDB mydb;
DFRobot_HuskyLens huskylens;
DFRobot_MLX90614  mlx90614;


// 主程序开始
void setup() {
	mPython.begin();
	display.setCursorLine(1);
	display.printLine("开始连接WiFi");
	myIot.wifiConnect("Huang", "88888888");
	while (!myIot.wifiStatus()) {yield();}
	display.setCursorLine(2);
	display.printLine("WiFi连接成功");
	myIot.init("iot.dfrobot.com.cn","aETlbrdGR","","-Po_x9dGRz",topics,1883);
	myIot.connect();
	while (!myIot.connected()) {yield();}
	display.setCursorLine(3);
	display.printLine("MQTT连接成功");
	delay(2000);
	display.fillScreen(0);
	mydb.setServerParameter("http://tinywebdb.appinventor.space/api", "huang11","658949d1");
	huskylens.beginI2CUntilSuccess();
	huskylens.writeAlgorithm(ALGORITHM_FACE_RECOGNITION);
}
void loop() {
	huskylens.request();
	if (huskylens.isAppearDirect(HUSKYLENSResultBlock)) {
		if (huskylens.isLearned(huskylens.readBlockCenterParameterDirect().ID)) {
			mind_s_XingMing = mydb.getTag((String(huskylens.readBlockCenterParameterDirect().ID)));
			display.setCursorLine(1);
			display.printLine(mind_s_XingMing);
			display.setCursorLine(2);
			display.printLine("请保持面向摄像头");
			display.setCursorLine(3);
			display.printLine("开始测温");
			display.setCursorLine(4);
			display.printLine((String("体温为：") + String(mlx90614.getObjectTempC())));
			myIot.publish(topic_0, (String(mind_s_XingMing) + String((String("的温度为：") + String(mlx90614.getObjectTempC())))));
			delay(3000);
		}
	}
}

