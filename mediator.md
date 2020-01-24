# Mediator pattern
## Many-to-many interaction

```ctc++

#include <iostream>
#include <set>

struct Condition{
	int RoadHazard;
};
class IMediator;

class ISubject{
	ISubject() = default;
	~ISubject() = default;
	virtual void sendMessage(IMediator* subject) = 0;
	virtual void receiveMessage(const Condition& data) = 0;
};

class IMediator{
    IMediator() = default; 
	~IMediator() = default;
    virtual void register(ISubject* subject) = 0;
    virtual void notify(const Condition& data) = 0;
};



class Mediator: IMediator{
	public:
	  Mediator();
	  ~Mediator();
    virtual void register(ISubject* subject) override;
    virtual void notify(const Condition& data) override;
	private:
    std::set<ISubject> registeredObjects;
};
Mediator::Mediator(){
// do nothing
}
Mediator::~Mediator(){
 // do nothing
}

void Mediator::register(ISubject* subject){
	if(registeredObjects.count(subject) == 0){
		registeredObjects.insert(subject);
	}
}

void Mediator::notify(const Condition& data){
	for(ISubject* x : 	for(ISubject* x : registeredObjects){
		x->receiveMessage(data)
	}	
} 

SomeImpClass{
	SomeImpClass() = default;
	~SomeImpClass() = default;
	virtual void funding();
	virtual void personnel();
}

SomeImpClass::funding{std::cout << "Funding being decided" << std::endl;}
SomeImpClass::personnel{std::cout << "persons assigned" << std::endl;}

class NationalParkService: SomeImpClass, ISubject{
	public:
		NationalParkService();
		NationalParkService(const Condition indata);
		~NationalParkService();
		virtual void sendMessage(IMediator* mediator) override;
		virtual void receiveMessage(const Condititon& data) override;
		void setData(const Condition& data) const;
		Condition data;	
}

NationalParkService::NationalParkService()
{
   // do noting
}

NationalParkService::NationalParkService(const Condition indata): data{indata}
{
   // do noting
}

NationalParkService::~NationalParkService(){
	// do noting
}

void NationalParkService::sendMessage(IMediator* mediator){
	mediator->notify(this->data);
}
void NationalParkService::receiveMessage(const Condititon& data){
	this->data = data;
}

class DepartmentTransportation: SomeImpClass, ISubject{
	public:
		DepartmentTransportation();
		DepartmentTransportation(const Condition indata);
		~DepartmentTransportation();
		virtual void sendMessage(IMediator* subject) override;
		virtual void receiveMessage() override;
		Condition data;	
}
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

void DepartmentTransportation::sendMessage(IMediator* mediator){
	mediator->notify(this->data);
}
void DepartmentTransportation::receiveMessage(const Condititon& data){
	this->data = data;
}

int main(){
    
    Mediator mediator;
    Condition pcond{10};
    NationalParkService park(pcond);
    DepartmentTransportation trans;
    mediator.register(&park);
    mediator.register(&trans);
    park.sendMessage(*mediator);
    std::cout << "trans.data.RoadHazard " << trans.data.RoadHazard << std::endl;
    std::cout << "park.data.RoadHazard " << park.data.RoadHazard << std::endl;
   
    return 0;
}

```

