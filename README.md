# one - to - many interaction

## Observer pattern 

```c++
#include <iostream>
#include <memory>
#include <set>

    struct WData {
  float temp;
  float humidity;
};

class Observer {
public:
  Observer() = default;
  virtual ~Observer() = default;
  virtual void update(const WData &data) = 0;
};

class Subject {
public:
  Subject() = default;
  virtual ~Subject() = default;
  virtual void registerObserver(std::shared_ptr<Observer> obs) = 0;
  virtual void unregisterObserver(std::shared_ptr<Observer> obs) = 0;
  virtual void notifyObserver() = 0;
};

class Communication {
public:
  Communication() = default;
  ~Communication() = default;
  virtual void sendData();
  virtual bool receiveData();
};
void Communication::sendData() {}
bool Communication::receiveData() {}

class DeptTransportation : public Communication, public Observer {
  void update(const WData &data) override;
};

void DeptTransportation::update(const WData &data) {
  std::cout << "Dept of transportatio received data" << std::endl;
  std::cout << "Temp: " << data.temp << " Humidity: " << data.humidity
            << std::endl;
}
class NationalParkService : public Communication, public Observer {
  void update(const WData &data) override;
};
void NationalParkService::update(const WData &data) {
  std::cout << "National Park Service received data" << std::endl;
  std::cout << "Temp: " << data.temp << " Humidity: " << data.humidity
            << std::endl;
}

class Weather : public Communication, public Subject {
public:
  Weather(float temp, float humidity);
  ~Weather();
  void registerObserver(std::shared_ptr<Observer> obs) override;
  void unregisterObserver(std::shared_ptr<Observer> obs) override;
  void notifyObserver() override;

private:
  WData m_data;
  std::set<std::shared_ptr<Observer>> m_observerPtrs;
};

Weather::Weather(float temp, float humidity) : m_data{temp, humidity} {
  //
}
Weather::~Weather() {}

void Weather::registerObserver(std::shared_ptr<Observer> obs) {
  if (m_observerPtrs.count(obs) == 0) {
    m_observerPtrs.insert(obs);
  }
}
void Weather::unregisterObserver(std::shared_ptr<Observer> obs) {
  if (m_observerPtrs.count(obs) != 0) {
    m_observerPtrs.erase(obs);
  }
}
void Weather::notifyObserver() {
  for (std::shared_ptr<Observer> ob : m_observerPtrs) {
    ob->update(m_data);
  }
}

int main() {

  std::shared_ptr<Communication> weather = std::make_shared<Weather>(70, 0.90);
  std::shared_ptr<Communication> transport =
      std::make_shared<DeptTransportation>();
  std::shared_ptr<Communication> park = std::make_shared<NationalParkService>();

  std::shared_ptr<Weather> temp;
  temp = std::dynamic_pointer_cast<Weather>(weather);
  temp->registerObserver(std::dynamic_pointer_cast<Observer>(transport));
  temp->registerObserver(std::dynamic_pointer_cast<Observer>(park));
  temp->notifyObserver();
  temp->unregisterObserver(std::dynamic_pointer_cast<Observer>(transport));
  temp->notifyObserver();
  return 0;
}
```
