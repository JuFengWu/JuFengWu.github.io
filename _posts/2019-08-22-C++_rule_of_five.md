---
title: C++ rule of five
author: Jufeng Wu
layout: post
---

----------------------
什麼是rule of five（五法則）?<br/> 
基本上就是幫你有寫出這五個其中一個東西的時候，剩下四個也最好複寫，雖然compiler會幫你作掉一些，但是有可能會出現不可預期的錯誤<br/>

在c++11 (modern c++)之前有三法則，分別是<br/>
1.copy construct<br/>
2.destructor <br/>
3.copy assignment operator<br/>
<br/>而c++11之後則是有五法則,分別是:<br/>
1.copy construct<br/>
2.destructor <br/>
3.copy assignment operator<br/>
4.move construct<br/>
5.move assignment operator<br/><br/>
因為c++11之後多了move這一個功能，所以多了兩個必須要寫的內容<br/>
基本上move是只複製指標，沒有複製內容，所以速度比較快，但是就從三法則變成五法則了！<br/>
五法則的用法如下：<br/>
### **copy construct:**
```
RuleOfFive(const RuleOfFive& other):RuleOfFive(other._a,*(other._b)){
	std::cout << "this is copy constructor!"<<std::endl;
	}
```
這很簡單，就是把東西copy過去<br/>
### **destructor:**
```
~RuleOfFive() {
		if (_b != nullptr) {
			delete _b;
			std::cout << "free it!" << std::endl;
		}
	}
```
就是把該free的東西free而已<br/>
### **copy assignment operator:**
```
RuleOfFive& operator = (const RuleOfFive &other) {
		std::cout << "this is copy operator =!!!"<<std::endl;
		
		RuleOfFive temp(other);

		*this = std::move(temp);

		return *this;
	}
```
這裡很有趣，有用到move operator，畢竟這樣速度會比較快<br/>
### **move construct:**
```
RuleOfFive(RuleOfFive&& other): _a{other._a},_b{other._b}{
		std::cout << "this is move constructor!"<<std::endl;
		other._b = nullptr;
	}
```
move construct在return物件回去的時候有時會用到，它return 回去時剛好外面創造物件，就有機會使用到這一個東西<br/>
有趣的是，我在visual studio 的時候它會自動用move 的方式return ，在linux用g＋＋的時候要額外`` return std::move(tm);``這樣才會呼叫這一個方法<br/>
### **move assignment operator:**
```
RuleOfFive& operator = (RuleOfFive && other) {
		std::cout << "this is move operator =!!!"<<std::endl;
		
		_a = other._a;
		delete _b;
		_b = other._b;
		other._b = nullptr;
		return *this;

	}
```
也可以這樣寫
```
RuleOfFive& operator = (RuleOfFive && other) {
		std::cout << "this is move operator =!!!"<<std::endl;
		
		_a = other._a;
		delete _b;
		_b = std::exchange(other._b, nullptr);
		return *this;

	}
```
但是這樣的話要 ``include <utility>`` <br/>
此外，要使用這一個方法，最快的方式是用``b=std::move(a)``，這樣就會呼叫這一個方法了<br/><br/>
在[這裡](https://github.com/JuFengWu/cpp_examples/tree/master/rule_of_five)有rule fo five的範例，裡面有rule of five的簡單範例，包含reference 和變數的使用<br/>
最後，要感謝跟我一起研究rule of five的techman robot同事們，才有這一篇文章
