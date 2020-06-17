---
title : "DataStructure"
category :
    - DataStructure
tag :
    - LinkedList
author_profile : true
sidebar_main : true
use_math : true
headr:
    teaser : 
---

# LinkedList

### LinkedList에 대해 알아보자

- 연산: 삽입, 삭제, 검색
- 구현방법: 배열, 연결리스트
배열: 크기가 고정되어 있어 삽입, 삭제 연산을 할때 다수의 데이터를 옮겨야한다.(최악의 경우 O(N))
리스트: 삽입, 삭제가 배열 보다 쉽고 리스트 길이 제한이 없다. 하지만 랜덤 Access가 불가능해 원하는 데이터를 Search하기 위해서 O(N)이 걸린다.(배열은 O(1))

---
**Linked List(Using Class)**

`code`
```cpp
#include<iostream>

using namespace std;

class List {
private:
	class Node {
	public:
		int data;
		Node *next;
	};
	void valid(int count);
	int count;
	Node* head = new Node;
public:
	List();
	
	int get(int idex);
	void add(int data);
	void add(int index, int data);
	int size();
	void set(int index, int data);
	void remove(int index);
	bool isEmpty();
};

List::List()
{
	head->next = NULL;
	List::count = 0;
}
int List::get(int index)
{
	try
	{
		valid(index);
	}
	catch (const char* msg) {
		cout << msg << '\n';
		return -1;
	}
	Node *temp = head;
	for (int i = 0; i <= index; i++) {
		temp = temp->next;
	}
	return temp->data;
}

void List::valid(int count) {
	if (count > List::count) {
		throw "Error : 유효하지 않는 index입니다.";
	}
}

int List::size() {
	return List::count;
}
void List::add(int data)
{
	Node *newNode = new Node;
	newNode->data = data;
	newNode->next = NULL;

	if (head->next == NULL) {
		head->next = newNode;
	}
	else {
		Node* temp = head;
		while (temp->next != NULL)
		{
			temp = temp->next;
		}
		temp->next = newNode;
	}
	List::count++;
}

void List::add(int index, int data)
{
	try
	{
		valid(index);
	}
	catch (const char* msg)
	{
		cout << msg << '\n';
		return;
	}
	Node* newNode = new Node;
	newNode->data = data;
	newNode->next = NULL;
	if (head->next == NULL) {
		head->next = newNode;
	}
	else {
		Node *temp = head;
		for (int i = 0; i < count; i++) {
			temp = temp->next;
		}
		newNode->next = temp->next;
		temp->next = newNode;
	}
	List::count++;
}
void List::set(int index, int data)
{
	try
	{
		valid(index);
	}
	catch (const char* msg)
	{
		cout << msg << '\n';
		return;
	}
	Node *temp = head;
	for (int i = 0; i <= index; i++) {
		temp = temp->next;
	}
	temp->data = data;
}
void List::remove(int index)
{
	if (head->next == NULL) {
		cout << "삭제할 Node가 없습니다." << '\n';
		return;
	}
	try
	{
		valid(index);
	}
	catch (const char* msg)
	{
		cout << msg << '\n';
		return;
	}
	Node *temp = head;
	Node* removeNode = head;
	
	for (int i = 0; i < index; i++) {
		temp = temp->next;
		removeNode = removeNode->next;
	}
	removeNode = removeNode->next;
	temp->next = removeNode->next;
	removeNode->next = NULL;
	delete removeNode;
	
	List::count--;

}
bool List::isEmpty()
{
	if (head->next == NULL)return true;
	else return false;
}

int main()
{
	List test;
	test.add(0);
	test.add(1);
	test.add(2);
	test.add(3);
	test.set(0, 10);
	test.remove(1);
	for (int i = 0; i < test.size(); i++) {
		cout << "data : " << test.get(i) << '\n';
	}
	cout << "size : " << test.size() << '\n';
	if(test.isEmpty()==true) cout << "isEmpty : " << "True" << '\n';
	else cout << "isEmpty : " << "No" << '\n';
	test.remove(10);
	return 0;
}
```
```sh

data : 10
data : 2
data : 3
size : 3
isEmpty : No
Error : 유효하지 않는 index입니다.
```
---

