#include<cstdlib>
using namespace std;
#include <ctime>
#include <iostream>
#include <string>
#ifndef __HEX__PUZZLE__TWO
#define __HEX__PUZZLE__TWO
#include "Header.h"

enum hexPieceEdges { NORTH, NORTHEAST, SOUTHEAST, SOUTH, SOUTHWEST, NORTHWEST };
enum tilePositionName { CENTERTILE, TOPTILE, TOPRIGHTTILE, BOTTOMRIGHTTILE,
	BOTTOMTILE, BOTTOMLEFTTILE, TOPLEFTTILE };

const int NUMOFTILES = 7;
const int NUMOFSIDES = 6;
const int SHUFFLECOUNT = 10;

class HexPuzzleTwo {
private:
	HexPiece gameTilesArr[NUMOFTILES];
	int tilesArrangementOnBoardArr[NUMOFTILES];

public:
	HexPuzzleTwo();
	int getTileEdgeVal(int tileID, int edgeVal);
	void displaySolution();
	void swapOutsideEdges(int tileID);
	void shuffleTileEdgeValues(int tileID);
	void createSolution();
	int getUnUsedEdgeValue(int tileID);
	bool isThisTileUnique(int tileID);
};

#endif

HexPuzzleTwo::HexPuzzleTwo() {
	//initialize the tileArrangement arr to 0's and the tile edge values to 0
	for (int i = 0; i < NUMOFTILES; i++) {
		tilesArrangementOnBoardArr[i] = 0;
		for (int j = 0; j < NUMOFSIDES; j++) {
			gameTilesArr[i].setEdgeValue(j, 0);
		}
	}
}
void HexPuzzleTwo::shuffleTileEdgeValues(int tileID) {
	int tempValue;
	int rndIndex;
	int lowerBoundIndex = 0;
	int upperBoundIndex = 5;
	//populate the center tile with edge values
	for (int i = 1; i <= NUMOFSIDES; i++)
		gameTilesArr[CENTERTILE].setEdgeValue(i - 1, i);
	srand((unsigned)time(NULL));
	for (int i = 0; i < SHUFFLECOUNT; i++) {
		rndIndex = rand() % (upperBoundIndex - lowerBoundIndex + 1) +
			lowerBoundIndex;
		tempValue = gameTilesArr[tileID].getEdgeValue(NORTH);
		gameTilesArr[tileID].setEdgeValue(NORTH,
			gameTilesArr[tileID].getEdgeValue(rndIndex));
		gameTilesArr[tileID].setEdgeValue(rndIndex, tempValue);
	}
}
void HexPuzzleTwo::swapOutsideEdges(int tileID) {
	int edge1Index;
	int edge2Index;
	int edge3Index;
	int tempIndex1;
	int tempIndex2;
	int lowerBoundIndex = 0;
	int upperBoundIndex = 5;
	int tempSideValue;
	//establish valid swappable indexes
	if (tileID == 1) {
		edge1Index = 5;
		edge2Index = 0;
		edge3Index = 1;
	}
	else if (tileID == 2) {
		edge1Index = 0;
		edge2Index = 1;
		edge3Index = 2;
	}
	else if (tileID == 3) {
		edge1Index = 1;
		edge2Index = 2;
		edge3Index = 3;
	}
	else if (tileID == 4) {
		edge1Index = 2;
		edge2Index = 3;
		edge3Index = 4;
	}
	else if (tileID == 5) {
		edge1Index = 3;
		edge2Index = 4;
		edge3Index = 5;
	}
	else if (tileID == 6) {
		edge1Index = 4;
		edge2Index = 5;
		edge3Index = 0;
	}
	srand((unsigned)time(NULL));
	tempIndex1 = rand() % (upperBoundIndex - lowerBoundIndex + 1) +
		lowerBoundIndex;
	tempIndex2 = rand() % (upperBoundIndex - lowerBoundIndex + 1) +
		lowerBoundIndex;
	while (!(tempIndex1 == edge1Index || tempIndex1 == edge2Index || tempIndex1
		== edge3Index)) {
		tempIndex1 = rand() % (upperBoundIndex - lowerBoundIndex + 1) +
			lowerBoundIndex;
	}
	while (!(tempIndex2 == edge1Index || tempIndex2 == edge2Index || tempIndex2
		== edge3Index || tempIndex1 == tempIndex2)) {
		tempIndex2 = rand() % (upperBoundIndex - lowerBoundIndex + 1) +
			lowerBoundIndex;
	}
	tempSideValue = gameTilesArr[tileID].getEdgeValue(tempIndex1);
	gameTilesArr[tileID].setEdgeValue(tempIndex1,
		gameTilesArr[tileID].getEdgeValue(tempIndex2));
	gameTilesArr[tileID].setEdgeValue(tempIndex2, tempSideValue);
}
int HexPuzzleTwo::getTileEdgeVal(int tileID, int edgeVal) {
	return gameTilesArr[tileID].getEdgeValue(edgeVal);
}
void HexPuzzleTwo::displaySolution() {
	string output = "";
	for (int tileCnt = 0; tileCnt < NUMOFTILES; tileCnt++) {
		cout << "Tile " << tilesArrangementOnBoardArr[tileCnt] << ": ";
		for (int edgeCnt = 0; edgeCnt < NUMOFSIDES; edgeCnt++) {
			cout << gameTilesArr[tileCnt].getEdgeValue(edgeCnt) << " ";
		}
		cout << endl;
	}
	cout << endl;
	output = output + " _" +
		to_string(gameTilesArr[TOPTILE].getEdgeValue(NORTH)) + "_ \n";
	output = output + " / \\ \n";
	output = output + " " +
		to_string(gameTilesArr[TOPTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[TOPTILE].getEdgeValue(NORTHEAST)) + " \n";
	output = output + " _" +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(NORTH)) + "_ / " +
		to_string(tilesArrangementOnBoardArr[1]) + " \\ _" +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(NORTH)) + "_ \n";
	output = output + " / \\ \\ / / \\ \n";
	output = output + " " +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(NORTHEAST)) + " " +
		to_string(gameTilesArr[TOPTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[TOPTILE].getEdgeValue(SOUTHEAST)) + " " +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(NORTHEAST)) + " \n";
	output = output + " / " + to_string(tilesArrangementOnBoardArr[6]) + " \\
		\\_" + to_string(gameTilesArr[TOPTILE].getEdgeValue(SOUTH)) + "_/ / " +
		to_string(tilesArrangementOnBoardArr[2]) + " \\ \n";
	output = output + " \\ / / " +
		to_string(gameTilesArr[CENTERTILE].getEdgeValue(NORTH)) + " \\ \\ / \n";
	output = output + " " +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(SOUTHEAST)) + " " +
		to_string(gameTilesArr[CENTERTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[CENTERTILE].getEdgeValue(NORTHEAST)) + " " +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(SOUTHEAST)) + " \n";
	output = output + " \\_" +
		to_string(gameTilesArr[TOPLEFTTILE].getEdgeValue(SOUTH)) + "_/ / " +
		to_string(tilesArrangementOnBoardArr[0]) + " \\ \\_" +
		to_string(gameTilesArr[TOPRIGHTTILE].getEdgeValue(SOUTH)) + "_/ \n";
	output = output + " / " +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(NORTH)) + " \\ \\ / / " +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(NORTH)) + " \\ \n";
	output = output + " " +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(NORTHEAST)) + " " +
		to_string(gameTilesArr[CENTERTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[CENTERTILE].getEdgeValue(SOUTHEAST)) + " " +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(NORTHEAST)) + " \n";
	output = output + " / " + to_string(tilesArrangementOnBoardArr[5]) + " \\
		\\_" + to_string(gameTilesArr[CENTERTILE].getEdgeValue(SOUTH)) + "_/ / " +
		to_string(tilesArrangementOnBoardArr[3]) + " \\ \n";
	output = output + " \\ / / " +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(NORTH)) + " \\ \\ / \n";
	output = output + " " +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(SOUTHEAST)) + " " +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(NORTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(NORTHEAST)) + " " +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(SOUTHEAST)) + " \n";
	output = output + " \\_" +
		to_string(gameTilesArr[BOTTOMLEFTTILE].getEdgeValue(SOUTH)) + "_/ / " +
		to_string(tilesArrangementOnBoardArr[4]) + " \\ \\_" +
		to_string(gameTilesArr[BOTTOMRIGHTTILE].getEdgeValue(SOUTH)) + "_/ \n";
	output = output + " \\ / \n";
	output = output + " " +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(SOUTHWEST)) + " " +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(SOUTHEAST)) + " \n";
	output = output + " \\_" +
		to_string(gameTilesArr[BOTTOMTILE].getEdgeValue(SOUTH)) + "_/ \n";
	cout << output;
}
void HexPuzzleTwo::createSolution() {
#pragma region declaration
	int centerEdgeVal1;
	int centerEdgeVal2;
	int tempTileID;
	int tempEdgeVal;
	int rndTileID;
	int rndEdgeVal;
	int lowerTileIDBound = 0;
	int upperTileIDBound = 6;
	int lowerEdgeBound = 0;
	int upperEdgeBound = 0;
	int connectingEdgeValues[] = { 1, 2, 3, 4, 5, 6 };
	bool isConnectingEdgesValuesEstablished = false;
#pragma endregion
	srand((unsigned)time(NULL));
#pragma region determine winning tile arrangement
	for (int i = 0; i < NUMOFTILES; i++)
		tilesArrangementOnBoardArr[i] = i + 1;
	for (int i = 0; i < SHUFFLECOUNT; i++) {
		rndTileID = rand() % (upperTileIDBound - lowerTileIDBound + 1) +
			lowerTileIDBound;
		tempTileID = tilesArrangementOnBoardArr[0];
		tilesArrangementOnBoardArr[0] = tilesArrangementOnBoardArr[rndTileID];
		tilesArrangementOnBoardArr[rndTileID] = tempTileID;
	}
#pragma endregion
	//randomize center tile
	shuffleTileEdgeValues(CENTERTILE);
#pragma region randomly assign connecting edge values
	lowerEdgeBound = 0;
	upperEdgeBound = 5;
	while (isConnectingEdgesValuesEstablished == false) {
		//randomize the connectingEdgeValues array
		for (int i = 0; i < SHUFFLECOUNT; i++) {
			rndEdgeVal = rand() % (upperEdgeBound - lowerEdgeBound + 1) +
				lowerEdgeBound;
			tempEdgeVal = connectingEdgeValues[0];
			connectingEdgeValues[0] = connectingEdgeValues[rndEdgeVal];
			connectingEdgeValues[rndEdgeVal] = tempEdgeVal;
		}
		//are the connecting edge values appropriate
		for (int i = 0; i < NUMOFSIDES; i++) {
			centerEdgeVal1 = gameTilesArr[CENTERTILE].getEdgeValue(i);
			centerEdgeVal2 = gameTilesArr[CENTERTILE].getEdgeValue((i + 1) %
				NUMOFSIDES);
			if (connectingEdgeValues[i] == centerEdgeVal1 ||
				connectingEdgeValues[i] == centerEdgeVal2) {
				isConnectingEdgesValuesEstablished = false;
				break;
			}
			else
				isConnectingEdgesValuesEstablished = true;
		}
	}
#pragma endregion
#pragma region randomly assign the outer edges of the tiles
	for (int i = 1; i < NUMOFTILES; i++) {
		gameTilesArr[i].setEdgeValue((i + 1) % NUMOFSIDES,
			connectingEdgeValues[i - 1]);
		gameTilesArr[i].setEdgeValue((i + 2) % NUMOFSIDES,
			gameTilesArr[CENTERTILE].getEdgeValue(i - 1));
		gameTilesArr[i].setEdgeValue((i + 3) % NUMOFSIDES,
			connectingEdgeValues[(i + 4) % NUMOFSIDES]);
		gameTilesArr[i].setEdgeValue((i + 4) % NUMOFSIDES,
			getUnUsedEdgeValue(i));
		gameTilesArr[i].setEdgeValue((i + 5) % NUMOFSIDES,
			getUnUsedEdgeValue(i));
		gameTilesArr[i].setEdgeValue((i + 6) % NUMOFSIDES,
			getUnUsedEdgeValue(i));
		//check if edge values for this tile are unique
		while (isThisTileUnique(i) == false) {
			//assign 0 the to outside edges
			gameTilesArr[i].setEdgeValue((i + 4) % NUMOFSIDES, 0);
			gameTilesArr[i].setEdgeValue((i + 5) % NUMOFSIDES, 0);
			gameTilesArr[i].setEdgeValue((i + 6) % NUMOFSIDES, 0);
			//reassign outside edge values
			gameTilesArr[i].setEdgeValue((i + 4) % NUMOFSIDES,
				getUnUsedEdgeValue(i));
			gameTilesArr[i].setEdgeValue((i + 5) % NUMOFSIDES,
				getUnUsedEdgeValue(i));
			gameTilesArr[i].setEdgeValue((i + 6) % NUMOFSIDES,
				getUnUsedEdgeValue(i));
		}
	}
#pragma endregion
}
int HexPuzzleTwo::getUnUsedEdgeValue(int tileID) {
	int edgeValues[] = { 1, 2, 3, 4, 5, 6 };
	int usedValues[] = { 0, 0, 0, 0, 0, 0 };
	int lowerIndex = 0;
	int upperIndex = 5;
	int rndIndex;
	//established which edges values are assigned
	for (int i = 0; i < NUMOFSIDES; i++) {
		if (gameTilesArr[tileID].getEdgeValue(i) > 0)
			usedValues[gameTilesArr[tileID].getEdgeValue(i) - 1] = 1;
	}
	//return a random edge value not yet assigned
	rndIndex = rand() % (upperIndex - lowerIndex + 1) + lowerIndex;
	while (usedValues[rndIndex] != 0)
		rndIndex = rand() % (upperIndex - lowerIndex + 1) + lowerIndex;
	return edgeValues[rndIndex];
}
bool HexPuzzleTwo::isThisTileUnique(int tileID) {
	int offset;
	int tileCnt;
	int sideCnt;
	bool thisTileIsUnique = false;
	sideCnt = 0;
	//compare the tile to determine if they are unique
	for (tileCnt = 0; tileCnt < tileID; tileCnt++) {
		thisTileIsUnique = false; //reset for this iteration
		//determine offset
		for (int i = 0; i < NUMOFSIDES; i++) {
			if (gameTilesArr[tileCnt].getEdgeValue(i) ==
				gameTilesArr[tileID].getEdgeValue(NORTH))
				offset = i;
		}
		//is the edge sequence unique for this tile
		for (int i = 0; i < NUMOFSIDES; i++) {
			if (gameTilesArr[tileCnt].getEdgeValue((i + offset) % NUMOFSIDES)
				!= gameTilesArr[tileID].getEdgeValue(i))
				thisTileIsUnique = thisTileIsUnique || true;
			else
				thisTileIsUnique = thisTileIsUnique || false;
		}
		if (thisTileIsUnique == false)
			return false;
	}
	return thisTileIsUnique;
}
