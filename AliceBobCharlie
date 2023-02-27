#include <Windows.h>
#include <string>
#include <vector>
#include <map>
#include <fstream>
#include <sstream>


//restarted on 27.02.2023 to avoid using regex - it works.
//in this module I will try to learn to do calculations. Maybe play around with data structures?

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam);

HWND hMainWindow;
HWND hCanvasWindow;
HINSTANCE hMainInstance;

HWND hLblOutputFirstName;
HWND hLblOutputNumber;
HWND hTxtInput;
HWND hButton;
HWND hButtonDel;

CHAR s_text_1[]{ "Random Text.." };
int posX = 0;
int posY = 0;

std::vector<HWND> windows;

#define IDC_TEXTBOX 1000
#define IDC_BUTTON 1001
#define IDC_BUTTON_DEL 1002

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE PrevInstance, LPSTR lpCmdLine, int nCmdShow) {

	MSG msg = { 0 };
	WNDCLASS wc = { 0 };
	wc.lpfnWndProc = WndProc;
	wc.hInstance = hInstance;
	wc.hbrBackground = (HBRUSH)(COLOR_BACKGROUND);
	wc.lpszClassName = TEXT("AliceBobCharlie");

	if (!RegisterClass(&wc))
		return 1;

	hMainWindow = CreateWindow(wc.lpszClassName, TEXT("AliceBobCharlie"), WS_OVERLAPPEDWINDOW | WS_HSCROLL | WS_VSCROLL | WS_VISIBLE,
		0, 0, 640, 480, 0, 0, hInstance, NULL);

	hMainInstance = wc.hInstance;

	while (GetMessage(&msg, NULL, 0, 0))
	{
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}

	return 0;
}

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	int offset = 0;
	std::wstring input;

	switch (message)
	{
		case WM_CREATE:
		{
			hMainWindow = hWnd;

			hCanvasWindow = CreateWindowEx(0, "STATIC", "bbb", WS_CHILD | WS_VISIBLE, posX, 0, 8000, 4800, hMainWindow,
				NULL, NULL, NULL);

			hTxtInput = CreateWindowEx(WS_EX_CLIENTEDGE, TEXT("EDIT"), s_text_1,
				WS_VISIBLE | WS_CHILD | ES_LEFT, 50, 50, 400, 25, hWnd, (HMENU)IDC_TEXTBOX, hMainInstance, NULL);

			hButton = CreateWindowEx(WS_EX_CLIENTEDGE, TEXT("BUTTON"), TEXT("Press me!"), WS_VISIBLE | WS_CHILD | WM_COPY | ES_LEFT,
				500, 50, 400, 25, hWnd, (HMENU)IDC_BUTTON, hMainInstance, NULL);

			hButtonDel = CreateWindowEx(WS_EX_CLIENTEDGE, TEXT("BUTTON"), TEXT("Delete all!"), WS_VISIBLE | WS_CHILD | WM_COPY | ES_LEFT, 
				500, 100, 400, 25, hWnd, (HMENU)IDC_BUTTON_DEL, hMainInstance, NULL);

			SCROLLINFO si;
			si.cbSize = sizeof(si);
			si.nPos = 0;
			si.nMax = 800;
			si.nPage = 400;
			si.fMask = SIF_ALL;
			si.nMin = 0;
			SetScrollInfo(hCanvasWindow, SB_HORZ, &si, TRUE);

			//vertical scrolling set up
SCROLLINFO sj;
sj.cbSize = sizeof(sj);
sj.nPos = 0;
sj.nMax = 1200;
sj.nPage = 400;
sj.fMask = SIF_ALL;
sj.nMin = 0;
SetScrollInfo(hCanvasWindow, SB_VERT, &sj, TRUE);

//break;

		}

		case WM_HSCROLL: {
			SCROLLINFO si;
			si.cbSize = sizeof(si);

			GetScrollInfo(hCanvasWindow, SB_HORZ, &si);
			switch (LOWORD(wParam)) {
			case SB_LINELEFT: {
				si.nPos -= 5;
			}break;
			case SB_LINERIGHT: {
				si.nPos += 5;
			}break;
			case SB_THUMBTRACK: {
				si.nPos = HIWORD(wParam);
			}break;
			};

			SetScrollInfo(hCanvasWindow, SB_HORZ, &si, TRUE);
			posX = -si.nPos;
			MoveWindow(
				hCanvasWindow,
				posX,
				posY,
				8000,
				4800,
				TRUE
			);
			break;
		}


		case WM_VSCROLL: {

			SCROLLINFO sj;
			sj.cbSize = sizeof(sj);

			GetScrollInfo(hCanvasWindow, SB_VERT, &sj);
			switch (LOWORD(wParam)) {
			case SB_LINEDOWN: {
				sj.nPos += 5;
			}break;
			case SB_LINEUP: {
				sj.nPos -= 5;
			}break;
			case SB_THUMBTRACK: {
				sj.nPos = HIWORD(wParam);
			}break;
			};

			SetScrollInfo(hCanvasWindow, SB_VERT, &sj, TRUE);
			posY = -sj.nPos;
			MoveWindow(
				hCanvasWindow,
				posX,
				posY,
				8000,
				4800,
				TRUE
			);
			break;
		}

		case WM_COMMAND: {

			if (LOWORD(wParam) == IDC_BUTTON)
			{
				input.resize(GetWindowTextLengthW(hTxtInput));
				GetWindowTextW(hTxtInput, input.data(), input.size() + 1);

				std::ifstream inputFile;
				inputFile.open(input.data());

				std::string line = "";

				std::map<std::string, int> data;

				while (std::getline(inputFile, line))
				{
					std::stringstream inputString(line);
					std::string firstName;
					int Number;
					std::string tempString;

					getline(inputString, firstName, ',');
					getline(inputString, tempString);
					Number = atoi(tempString.c_str());

					if (data.count(firstName) == 0){
					data[firstName] = Number;
					}
					else {
						data[firstName] += Number;
					}




				}

				for (const auto& key : data) {

					std::string keyfirst = key.first;
					std::string keysecond = std::to_string(key.second);

					hLblOutputFirstName = CreateWindowEx(WS_EX_STATICEDGE, TEXT("EDIT"), keyfirst.c_str(), WS_VISIBLE | WS_CHILD | ES_READONLY |
						ES_LEFT, 50, 200 + offset * 26, 100, 25, hCanvasWindow,
						(HMENU)IDC_TEXTBOX, hMainInstance, NULL);
					hLblOutputNumber = CreateWindowEx(WS_EX_STATICEDGE, TEXT("EDIT"), keysecond.c_str(), WS_VISIBLE | WS_CHILD | ES_READONLY |
						ES_LEFT, 250, 200 + offset * 26, 100, 25, hCanvasWindow,
						(HMENU)IDC_TEXTBOX, hMainInstance, NULL);
					++offset;
				}


			}

			else if (LOWORD(wParam) == IDC_BUTTON_DEL)
			{
				;
			}break;

		}default:
			return DefWindowProc(hWnd, message, wParam, lParam);

	}
	return 0;
}
