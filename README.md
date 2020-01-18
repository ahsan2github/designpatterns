Common Design Patterns, how to send different observers different data

# one-to-many interaction

## Observer pattern 

```c++
#include <iostream>
#include <set>

struct WData{
  float temp;
  float humidity;
};

class Observer{
  public:
    Observer() = default;
    virtual ~Observer() = default;
  	virtual void update(const WData& data) = 0;
  };

class Subject{
  public:
    Subject() = default;
  	virtual ~Subject() = default;
  	virtual void registerObserver(Observer* obs) = 0;
  	virtual void unregisterObserver(Observer* obs) = 0;
  	virtual void notifyObserver() = 0;
  };

class Communication{
  public:
  	Communication() = default;
  	~Communication() = default;
  	virtual void sendData();
  	virtual bool receiveData();	
};
void Communication::sendData(){ }
bool Communication::receiveData(){ }

class DeptTransportation: public Communication, public Observer{
  void update(const WData& data) override;
};  
 
void DeptTransportation::update(const WData& data) {
  std::cout << "Dept of transportatio received data" << std::endl;
  std::cout << "Temp: " << data.temp << "Humidity: " << data.humidity << std::endl;
}  
class NationalParkService: public Communication, public Observer{
  void update(const WData& data) override;
};  
void NationalParkService::update(const WData& data){
  std::cout << "National Park Service received data" << std::endl;
  std::cout << "Temp: " << data.temp << "Humidity: " << data.humidity << std::endl;
} 
  
class Weather: public Communication, public Subject{
	public:
  	Weather(float temp, float humidity);
  	~Weather();
  	void registerObserver(Observer* obs) override;
  	void unregisterObserver(Observer* obs) override;
  	void notifyObserver() override;
  private:
  	WData m_data;
  	std::set<Observer*> m_observerPtrs;
};

Weather::Weather(float temp, float humidity):m_data{temp, humidity}
  {
    // 
  }
Weather::~Weather(){}
  
void Weather::registerObserver(Observer* obs){
 	if(m_observerPtrs.count(obs) == 0) {
    m_observerPtrs.insert(obs);
  }
}
void Weather::unregisterObserver(Observer* obs){
  if(m_observerPtrs.count(obs) != 0){
    m_observerPtrs.erase(obs);
  }
}
void Weather::notifyObserver(){
  for(Observer* ob : m_observerPtrs){
    ob->update(m_data);
  }
} 
               
int main(){
  Weather weather{70, 0.90};
  DeptTransportation transport;
  NationalParkService park;
  weather.registerObserver(&transport);
  weather.registerObserver(&park);
  weather.notifyObserver();  
  weather.unregisterObserver(&transport);
  weather.notifyObserver();
  return 0;
}               
```

