# Brick-ball-project
This is console based project which name is "Brick Ball Breaker".
#include <iostream>
#include <conio.h>  
#include <vector>
#include <windows.h>  
using namespace std;
const int width = 20;
const int height = 20;
const int paddleWidth = 10;
const char paddleChar = '_';
const char ballChar = 'O';
const char brickChar = '=';
class BrickBreaker {
public:
	    int ballX, ballY, ballDirX, ballDirY;
    int paddleX;
    bool gameOver;
    vector<vector<bool> >bricks;
    BrickBreaker() {
        reset();
    }
    void run() {
        while (!gameOver) {
            draw();
            input();
            logic();
            Sleep(20);  
        }
    }
    void reset() {
        ballX = width / 2;
        ballY = height / 2;
        ballDirX = 1;
        ballDirY = 1;
        paddleX = (width - paddleWidth) / 2;
        gameOver = false;
        bricks = vector<vector<bool> >(5, vector<bool>(width, false));
        for (int i = 0; i < 5; i++) {
            for (int j = 0; j < width; j++) {
                bricks[i][j] = true;		}			}
            }
    void clearScreen() {
        COORD topLeft = {0, 0};
        HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
        CONSOLE_SCREEN_BUFFER_INFO screen;
        DWORD written;
        GetConsoleScreenBufferInfo(console, &screen);
        FillConsoleOutputCharacterA(console, ' ', screen.dwSize.X * screen.dwSize.Y, topLeft, &written);
        FillConsoleOutputAttribute(console, FOREGROUND_GREEN | FOREGROUND_RED | FOREGROUND_BLUE,
                                   screen.dwSize.X * screen.dwSize.Y, topLeft, &written);
        SetConsoleCursorPosition(console, topLeft);
    }
    void draw() {
        clearScreen();
   for (int i = 0; i < width + 2; i++) {
            cout << '-';
        }
        cout << endl;
        for (int i = 0; i < height; i++) {
            for (int j = 0; j < width; j++) {
                if (j == 0) {
                    cout << '|';  
                }
                if (i < 5 && bricks[i][j]) {
                    cout << brickChar;
                } else if (i == ballY && j == ballX) {
                    cout << ballChar;
                } else if (i == height - 1 && j >= paddleX && j < paddleX + paddleWidth) {
                    cout << paddleChar;
                } else {
                    cout << " ";
                }
                if (j == width - 1) {
                    cout << '|';  
                }			}
            cout << endl;
        }
        for (int i = 0; i < width + 2; i++) {
            cout << '_';
        }	cout << endl;
    }
    void input() {
        if (_kbhit()) {
            switch (_getch()) {
                case 'a':
                    if (paddleX > 0) {
                        paddleX--;
                    }	break;
                case 'd':
                    if (paddleX < width - paddleWidth) {
                        paddleX++;
                    }	break;
                case 'q':
                    gameOver = true;
                    break;
            }				}			}
    void logic() {
        ballX += ballDirX;
        ballY += ballDirY;
        if (ballX == 0 || ballX == width - 1) {
            ballDirX = -ballDirX;
        }
        if (ballY == 0) {
            ballDirY = -ballDirY;
        }
        if (ballY == height - 1) {
            if (ballX >= paddleX && ballX < paddleX + paddleWidth) {
                ballDirY = -ballDirY;
            } else {
                gameOver = true;
            }		}
        if (ballY < 5 && bricks[ballY][ballX]) {
            bricks[ballY][ballX] = false;
            ballDirY = -ballDirY;
        }		}			};
int main() {
	int option;
	cout<<"WELCOME TO BRICK BALL GAME "<<endl<<endl;
	cout<<"IF you want to start this game please enter 1 "<<endl;
	cin>>option;
	switch(option){
		case 1 :
	BrickBreaker game;
    game.run();
    break;
    }
    return 0;
}
