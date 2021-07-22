# Контейнеры map и set
#include <iostream>
#include <cstdlib>
#include <iomanip>
#include <algorithm>
#include <vector>
#include <map>
#include <iterator>

using namespace std;

inline void zapolnenie_vec(vector<int> &vec1, int& size);
inline void zapolnenie_map(map<int,int>& map1, int& size);
void vivod_vec(vector<int>& vec1);
void vivod_map(map<int, int>& map1);
void erase_vec(vector<int>& vec1, int size, int size_erase);
void erase_map(map<int, int>& map1, int size, int size_erase);
void synchronization(vector<int>& vec1, map<int, int>& map1);
bool map_find(map<int, int>& map1, int f);
int main()
{
	system("chcp 1251>nul");
	srand(2);

	int size = rand() % 5 + 10;
	int size_erase = rand() % (size - 5) + 2;
	vector<int> vec1;
	map<int,int> map1;

	zapolnenie_vec(vec1,size);
	zapolnenie_map(map1, size);

	cout << "Ввод:" << endl;
	vivod_vec(vec1);
	vivod_map(map1);

	erase_vec(vec1, size, size_erase);
	erase_map(map1, size, size_erase);

	cout << "Результаты после удаления случайных элементов:" << endl;
	vivod_vec(vec1);
	vivod_map(map1);
	
	synchronization(vec1,map1);

	cout << "Результаты синхронизации:" << endl;
	vivod_vec(vec1);
	vivod_map(map1);

	system("pause>nul");
	return 0;
}
bool map_find(map<int, int>& map1, int f)
{
	for (map<int, int>::iterator it = map1.begin(); it != map1.end(); it++)
	{
		if (it->second == f)
		{
			return false;
		}
	}
	return true;
}
void synchronization(vector<int>& vec1, map<int, int>& map1)
{
	vector<int>::iterator it_vec1;
	map<int, int>::iterator it_map1;
	for (it_vec1 = vec1.begin();it_vec1 != vec1.end();)
	{
		if (map_find(map1,*it_vec1))
		{
			it_vec1=vec1.erase(remove(vec1.begin(), vec1.end(), *it_vec1));
			it_vec1 = vec1.begin();
		}
		else it_vec1++;
	}
	for (it_map1 = map1.begin();it_map1 != map1.end();)
	{
		if (find(vec1.begin(), vec1.end(), it_map1->second) == vec1.end())
		{
			it_map1 = map1.erase(it_map1);
		}
		else it_map1++;
	}
}
void erase_vec(vector<int>& vec1,int size, int size_erase)
{
	vector<int>::iterator era1;
	for (int i = 0; i < size_erase; i++)
	{
		era1 = vec1.begin();
		era1 = era1 + rand() % size;
		vec1.erase(era1);
		size--;
	}
}
void erase_map(map<int, int>& map1, int size, int size_erase)
{
	map<int,int>::iterator era1;
	for (int i = 0; i < size_erase; i++)
	{
		era1 = map1.begin();
		advance(era1, rand() % size);
		map1.erase(era1);
		size--;
	}
}
void zapolnenie_vec(vector<int>& vec1, int& size)
{
	for (int i = 0; i < size; i++)
	{
		vec1.push_back(rand()%9+1);
	}
}
void zapolnenie_map(map<int, int>& map1, int& size)
{
	for (int i = 0; i < size; i++)
	{
		map1.insert(make_pair(i,rand() % 9 + 1));
	}
}
void vivod_vec(vector<int>& vec1)
{
	vector<int>::iterator it_vec = vec1.begin();
	cout << "Значение в vector: " << endl;
	while (it_vec != vec1.end())
	{
		cout <<"Значение - "<< *it_vec++ << endl;
	}
	cout << endl;
}
void vivod_map(map<int, int>& map1)
{
	map<int,int>::iterator it_map = map1.begin();
	cout << "Значение в map: "<<endl;
	while (it_map != map1.end())
	{
		cout <<"Ключ - "<< setw(2) << it_map->first << "    Значение - " << it_map->second << endl;
		it_map++;
	}
	cout << endl;
}
