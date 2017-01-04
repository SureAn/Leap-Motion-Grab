# Leap-Motion-Grab
//To Grab Leap Motion

#include <iostream>
#include <cstring>
#include "Leap.h"
#include<fstream>


using namespace Leap;
using namespace std;

class SampleListener : public Listener {
  public:
    virtual void onInit(const Controller&);
    virtual void onConnect(const Controller&);
    virtual void onDisconnect(const Controller&);
    virtual void onExit(const Controller&);
    virtual void onFrame(const Controller&);
    virtual void onFocusGained(const Controller&);
    virtual void onFocusLost(const Controller&);
    virtual void onDeviceChange(const Controller&);
    virtual void onServiceConnect(const Controller&);
    virtual void onServiceDisconnect(const Controller&);
    virtual void onServiceChange(const Controller&);
    virtual void onDeviceFailure(const Controller&);
    virtual void onLogMessage(const Controller&, MessageSeverity severity, int64_t timestamp, const char* msg);
};

const std::string fingerNames[] = {"Thumb", "Index", "Middle", "Ring", "Pinky"};
const std::string boneNames[] = {"Metacarpal", "Proximal", "Middle", "Distal"};

void SampleListener::onInit(const Controller& controller) {
	std::cout << "Initialized" << std::endl;
	ofstream out("SJ1.txt", ios::app);
	out << "Initialized" << endl;
	out.close();
}

void SampleListener::onConnect(const Controller& controller) {
  std::cout << "Connected" << std::endl;
}

void SampleListener::onDisconnect(const Controller& controller) {
  // Note: not dispatched when running in a debugger. 
  std::cout << "Disconnected" << std::endl;
}

void SampleListener::onExit(const Controller& controller) {
  std::cout << "Exited" << std::endl;
}

void SampleListener::onFrame(const Controller& controller) {
  // Get the most recent frame and report some basic information
  const Frame frame = controller.frame();
  std::cout << "Frame id: " << frame.id()
            << ", timestamp: " << frame.timestamp()
            << ", hands: " << frame.hands().count()
            << ", extended fingers: " << frame.fingers().extended().count() << std::endl;
		 ofstream out("SJ1.txt", ios::app);//test
		out << "Frame id: "<< frame.id()
			<< ", timestamp: " << frame.timestamp()
			<< ", hands: " << frame.hands().count()
			<< ", extended fingers: " << frame.fingers().extended().count() << std::endl;
		out.close();//test

  HandList hands = frame.hands();
  for (HandList::const_iterator hl = hands.begin(); hl != hands.end(); ++hl) {
    // Get the first hand
    const Hand hand = *hl;
    std::string handType = hand.isLeft() ? "Left hand" : "Right hand";
    std::cout << std::string(2, ' ') << handType << ", id: " << hand.id()
              << ", palm position: " << hand.palmPosition() << std::endl;
	ofstream out("SJ1.txt", ios::app);//test
	out << std::string(2, ' ') << handType << ", id: " << hand.id()
		<< ", palm position: " << hand.palmPosition() << std::endl;
	//out.close();//test


    // Get the hand's normal vector and direction
    const Vector normal = hand.palmNormal();
    const Vector direction = hand.direction();

    // Calculate the hand's pitch, roll, and yaw angles
    std::cout << std::string(2, ' ') <<  "pitch: " << direction.pitch() * RAD_TO_DEG << " degrees, "
              << "roll: " << normal.roll() * RAD_TO_DEG << " degrees, "
              << "yaw: " << direction.yaw() * RAD_TO_DEG << " degrees" << std::endl;
	ofstream out1("SJ1.txt", ios::app);//test
	out1 << std::string(2, ' ') << "pitch: " << direction.pitch() * RAD_TO_DEG << " degrees, "
		<< "roll: " << normal.roll() * RAD_TO_DEG << " degrees, "
		<< "yaw: " << direction.yaw() * RAD_TO_DEG << " degrees" << std::endl;
	out1.close();//test


    // Get the Arm bone
    Arm arm = hand.arm();
    std::cout << std::string(2, ' ') <<  "Arm direction: " << arm.direction()
              << " wrist position: " << arm.wristPosition()
              << " elbow position: " << arm.elbowPosition() << std::endl;
	ofstream out2("SJ1.txt", ios::app);//test
	out2 << std::string(2, ' ') << "Arm direction: " << arm.direction()
		<< " wrist position: " << arm.wristPosition()
		<< " elbow position: " << arm.elbowPosition() << std::endl;
	out2.close();//test

    // Get fingers
    const FingerList fingers = hand.fingers();
    for (FingerList::const_iterator fl = fingers.begin(); fl != fingers.end(); ++fl) {
      const Finger finger = *fl;
      std::cout << std::string(4, ' ') <<  fingerNames[finger.type()]
                << " finger, id: " << finger.id()
                << ", length: " << finger.length()
                << "mm, width: " << finger.width() << std::endl;
	  ofstream out("SJ1.txt", ios::app);//test
	  out << std::string(4, ' ') << fingerNames[finger.type()]
		  << " finger, id: " << finger.id()
		  << ", length: " << finger.length()
		  << "mm, width: " << finger.width() << std::endl;
	  out.close();//test

      // Get finger bones
      for (int b = 0; b < 4; ++b) {
        Bone::Type boneType = static_cast<Bone::Type>(b);
        Bone bone = finger.bone(boneType);
        std::cout << std::string(6, ' ') <<  boneNames[boneType]
                  << " bone, start: " << bone.prevJoint()
                  << ", end: " << bone.nextJoint()
                  << ", direction: " << bone.direction() << std::endl;
		ofstream out("SJ1.txt", ios::app);//test
		out << std::string(6, ' ') << boneNames[boneType]
			<< " bone, start: " << bone.prevJoint()
			<< ", end: " << bone.nextJoint()
			<< ", direction: " << bone.direction() << std::endl;
		out.close();//test
      }
    }
  }

  if (!frame.hands().isEmpty()) {
    std::cout << std::endl;
  }

}

void SampleListener::onFocusGained(const Controller& controller) {
  std::cout << "Focus Gained" << std::endl;
  ofstream out("SJ1.txt", ios::app);//test
  out << "Focus Gained" << std::endl;
  out.close();//test
}

void SampleListener::onFocusLost(const Controller& controller) {
  std::cout << "Focus Lost" << std::endl;
  ofstream out("SJ1.txt", ios::app);//test
  out << "Focus Lost" << std::endl;
  out.close();//test
}

void SampleListener::onDeviceChange(const Controller& controller) {
  std::cout << "Device Changed" << std::endl;
  ofstream out("SJ1.txt", ios::app);//test
  out << "Device Changed" << std::endl;
  out.close();//test
  const DeviceList devices = controller.devices();

  for (int i = 0; i < devices.count(); ++i) {
    std::cout << "id: " << devices[i].toString() << std::endl;
	ofstream out("SJ1.txt", ios::app);//test
	out << "id: " << devices[i].toString() << std::endl;
	out.close();//test

    std::cout << "  isStreaming: " << (devices[i].isStreaming() ? "true" : "false") << std::endl;
	ofstream out1("SJ1.txt", ios::app);//test
	out1 << "  isStreaming: " << (devices[i].isStreaming() ? "true" : "false") << std::endl;
	out1.close();//test

    std::cout << "  isSmudged:" << (devices[i].isSmudged() ? "true" : "false") << std::endl;
	ofstream out2("SJ1.txt", ios::app);//test
	out2 << "  isSmudged:" << (devices[i].isSmudged() ? "true" : "false") << std::endl;
	out2.close();//test

    std::cout << "  isLightingBad:" << (devices[i].isLightingBad() ? "true" : "false") << std::endl;
	ofstream out3("SJ1.txt", ios::app);//test
	out3 << "  isLightingBad:" << (devices[i].isLightingBad() ? "true" : "false") << std::endl;
	out3.close();//test
  }
}

void SampleListener::onServiceConnect(const Controller& controller) {
  std::cout << "Service Connected" << std::endl;
}

void SampleListener::onServiceDisconnect(const Controller& controller) {
  std::cout << "Service Disconnected" << std::endl;
}

void SampleListener::onServiceChange(const Controller& controller) {
  std::cout << "Service Changed" << std::endl;
}

void SampleListener::onDeviceFailure(const Controller& controller) {
  std::cout << "Device Error" << std::endl;
  const Leap::FailedDeviceList devices = controller.failedDevices();

  for (FailedDeviceList::const_iterator dl = devices.begin(); dl != devices.end(); ++dl) {
    const FailedDevice device = *dl;
    std::cout << "  PNP ID:" << device.pnpId();
    std::cout << "    Failure type:" << device.failure();
  }
}

void SampleListener::onLogMessage(const Controller&, MessageSeverity s, int64_t t, const char* msg) {
  switch (s) {
  case Leap::MESSAGE_CRITICAL:
    std::cout << "[Critical]";
    break;
  case Leap::MESSAGE_WARNING:
    std::cout << "[Warning]";
    break;
  case Leap::MESSAGE_INFORMATION:
    std::cout << "[Info]";
    break;
  case Leap::MESSAGE_UNKNOWN:
    std::cout << "[Unknown]";
  }
  std::cout << "[" << t << "] ";
  std::cout << msg << std::endl;
}

int main(int argc, char** argv) {
  // Create a sample listener and controller
  SampleListener listener;
  Controller controller;

  // Have the sample listener receive events from the controller
  controller.addListener(listener);

  if (argc > 1 && strcmp(argv[1], "--bg") == 0)
    controller.setPolicy(Leap::Controller::POLICY_BACKGROUND_FRAMES);

  controller.setPolicy(Leap::Controller::POLICY_ALLOW_PAUSE_RESUME);



  // Keep this process running until Enter is pressed
  std::cout << "Press Enter to quit, or enter 'p' to pause or unpause the service..." << std::endl;
 
  bool paused = false;
  while (true) {
    char c = std::cin.get();
    if (c == 'p') {
      paused = !paused;
      controller.setPaused(paused);
      std::cin.get(); //skip the newline

    }
    else
      break;
  }

  // Remove the sample listener when done
  controller.removeListener(listener);
  
  return 0;
}
