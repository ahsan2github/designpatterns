# Mediator pattern
## Many-to-many interaction

```c++
#include <iostream>
#include <set>
#include <memory>

struct Condition{
	int RoadHazard;
};

class IMediator;
class ISubject;

class ISubject{
    public:
	ISubject() = default;
	~ISubject() = default;
	virtual void sendMessage(std::shared_ptr<IMediator> mediator) = 0;
	virtual void receiveMessage(const Condition& data) = 0;
};


class IMediator{
    public:
    IMediator() = default; 
	~IMediator() = default;
	 virtual void registerSub(std::shared_ptr<ISubject> subject) = 0;
    virtual void notify(const Condition& data) = 0;
};



class Mediator: public IMediator{
	public:
	  Mediator();
	  ~Mediator();
      virtual void registerSub(std::shared_ptr<ISubject> subject) override;
      virtual void notify(const Condition& data) override;
	private:
      std::set< std::shared_ptr<ISubject> > registeredObjects;
};

Mediator::Mediator(){
// do nothing
}

Mediator::~Mediator(){
 // do nothing
}

void Mediator::registerSub(std::shared_ptr<ISubject> subject){
	if(registeredObjects.find(subject) == registeredObjects.end() ){
		registeredObjects.insert(subject);
	}
}

void Mediator::notify(const Condition& data){
    std::cout << "Mediator notify method called " << std::endl;
	for(std::shared_ptr<ISubject> x : registeredObjects){
		x->receiveMessage(data);
	}	
} 

class SomeImpClass{
    public:
	    SomeImpClass() = default;
	    ~SomeImpClass() = default;
	    virtual void funding();
	    virtual void personnel();
};

void SomeImpClass::funding(){std::cout << "Funding being decided" << std::endl;}
void SomeImpClass::personnel(){std::cout << "persons assigned" << std::endl;}

class NationalParkService: public  SomeImpClass, public ISubject{
	public:
		NationalParkService();
		NationalParkService(const Condition indata);
		~NationalParkService();
		virtual void sendMessage(std::shared_ptr<IMediator> mediator) override;
		virtual void receiveMessage(const Condition& data) override;
		Condition data;	
};

NationalParkService::NationalParkService()
{
   // do noting
}

NationalParkService::NationalParkService(const Condition indata): data{indata}
{
   std::cout << "initialized data (NationalParkService) " << this-> data.RoadHazard << std::endl;
}

NationalParkService::~NationalParkService(){
	// do noting
}

void NationalParkService::sendMessage(std::shared_ptr<IMediator> mediator){
	mediator->notify(this->data);
}
void NationalParkService::receiveMessage(const Condition& data){
	this->data = data;
}

class DepartmentTransportation: public SomeImpClass, public ISubject{
	public:
		DepartmentTransportation();
		DepartmentTransportation(const Condition& indata);
		~DepartmentTransportation();
		virtual void sendMessage(std::shared_ptr<IMediator> subject) override;
		virtual void receiveMessage(const Condition& data) override;
		Condition data;	
};

DepartmentTransportation::DepartmentTransportation()
{
   // do noting
}
DepartmentTransportation::DepartmentTransportation(const Condition& indata): data{indata}
{
   // do noting
}

DepartmentTransportation::~DepartmentTransportation(){
	// do noting
}

void DepartmentTransportation::sendMessage(std::shared_ptr<IMediator> mediator){
	mediator->notify(this->data);
}
void DepartmentTransportation::receiveMessage(const Condition& data){
	this->data = data;
}

int main(){
    
    std::shared_ptr<IMediator> mediator = std::make_shared<Mediator>();
    Condition pcond{10};
    std::shared_ptr<SomeImpClass> park = std::make_shared<NationalParkService>(pcond);
    std::shared_ptr<SomeImpClass> trans = std::make_shared<DepartmentTransportation>();
    mediator->registerSub(std::dynamic_pointer_cast<ISubject>(park));
    mediator->aregisterSub(std::dynamic_pointer_cast<ISubject>(trans));
    std::dynamic_pointer_cast<ISubject>(park)->sendMessage(mediator);
    std::cout << "trans->data.RoadHazard " << std::dynamic_pointer_cast<DepartmentTransportation>(trans)->data.RoadHazard << std::endl;
    std::cout << "park->data.RoadHazard " << std::dynamic_pointer_cast<NationalParkService>(park)->data.RoadHazard << std::endl;
   
    return 0;
}


```

