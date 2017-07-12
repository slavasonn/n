#include "main.h"
int main() {
	House House1;
	Position pos1;
	Position pos2;
	Position pos3;
	Map Image(10,20);
    int house_number = 1;
    setlocale(LC_ALL, "");
	ifstream file;
    file.open("config.txt");
    string line;
    const char* temp;
    char *str1;
    int k = 0;
    double  parametrs[14];
    if (file.is_open()) {
		while (file.good()) {
			getline (file,line);
			temp = line.c_str();
			str1 = (char*)temp;
			//printf("%s\n", str1);
			char* pch = strtok (str1," ");
				while (pch != NULL) {
					parametrs[k] = atof(pch);
					//std::cout << (pch)  << "\n";
					pch = strtok (NULL, " ");
					k++; 
				}
			}
    file.close();
	}
    int Hhight = (pos1.getLatitude()-pos2.getLatitude())/K1*1;
    int Hwidth = (pos2.getLongtitude()-pos1.getLongtitude())/K2*1;
    House1.setHight(Hhight);
    House1.setWidth(Hwidth);
    int dist1 = (pos3.getLatitude()-Image.return_coord(1))/K1;
    int dist2 = (pos3.getLongtitude()-Image.return_coord(2))/K2;
    printf("%f%f", K1, K2);
    House1.Draw(Image, dist1, dist2);
    Image.save("Image.pnm");
   // free(Image);
    ///free(House1);
    char c;
    scanf("%c", &c);
    return 0;
}

#include "main1.h"
#include <malloc.h>
Map::Map(int n, int m) {
    int i,j;
    x_max=m;
    y_max=n;
    p = (int**) malloc(n*m*sizeof(int));
    for(i=0;i<n;i++) {
        for(j=0;j<m;j++)
        *(p+i*m+j) = 0;
    }
}

void Map::show() {
    int i,j;
    for(i=0;i<y_max;i++) {
        for(j=0;j<x_max;j++) {
            printf("%d", *(p + i*x_max+j));
        }
        printf("\n");
    }
}



float Map::return_coord(int t) {
    switch (t) {
    case 1:
        return lat1;
        break;
    case 2:
        return longt1;
        break;
    case 3:
        return lat2;
        break;
    case 4:
        return longt2;
        break;
    }
}

int Map::save(char *fname) {
	ofstream fout;//Запись файла, создали объект
	fout.open(fname);
	if (fout.is_open() == 0) { //1 -ok, 0- bad
		cout << "Error!" << endl;
		return 0;
	}
	fout << "P3" << endl;
	fout << "# This is an image" << endl;
	fout << Px << " " << Py << " " << endl;
	for (int i = 0; i < Px; i++) {
		for (int j = 0; j < Py; j++) {
			fout << p[i][j];

		}
		fout << endl;

	}
	fout.close();
}

#include "main1.h"

Position::Position(){
    latitude = 0;
    longtitude = 0;
}

void Position::setLatitude(float t_lat){
    latitude = t_lat;
}

void Position::setLongtitude(float t_longt){
    longtitude = t_longt;
}

float Position::getLatitude(){
    return latitude;
}

float Position::getLongtitude(){
    return longtitude;
}

#include "main1.h"
Position poss;
House::House() {
    num_corners = 0;
    hight = 0;
    width = 0;
	poss.setLatitude(0);
	poss.setLongtitude(0);
    for(int i = 0; i<4; i++) {
        p[i] = poss.getLatitude();
        p[i] = poss.getLongtitude();

    }

}


void House::add_Corner(Position t_p) {
    p[num_corners] = t_p;
    num_corners++;
}

void House::setHight(int t_hight) {
    hight = t_hight;
}

void House::setWidth(int t_width) {
    width = t_width;
}

#include <iostream>
#include <string>
#include <fstream>
#include <istream>
using namespace std;
#define Px 950
#define Py 616
class Map {
	int x_max;
	int y_max;
	int** p;
	float longt2;
	float lat1;
	float longt1;
	float lat2;
public:
	Map(int n, int m);
	void show();
	float return_coord(int t);
	int save(char* fname);
	~Map();
};

class Position {
	float longtitude;
	float latitude;
public:
	Position();
	void setLatitude(float t_lat);
	void setLongtitude(float t_longt);
	float getLatitude();
	float getLongtitude();

};

class House {
	int num_corners;
	double hight;
	double width;
	int p[];
public:
	House();
	void add_Corner(Position t_p);
	void setHight(int t_hight);
	void setWidth(int t_width);
	void Draw(Map x1, int x2, int x3);
};
