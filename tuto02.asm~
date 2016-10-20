.386
.model flat, stdcall
option casemap :none
include C:\masm32\include\windows.inc
include C:\masm32\include\user32.inc
include C:\masm32\include\kernel32.inc
includelib C:\masm32\lib\user32.lib
includelib C:\masm32\lib\kernel32.lib

WinMain proto :DWORD, :DWORD, :DWORD, :DWORD

.data
	ClassName	db "WinClass", 0
	AppName		db "Fernando's First Window", 0
	EditClassName 	db "edit", 0
	ButtonText 	db "ButtonString", 0
	ButtonClassName db "button", 0
	HelloWorld 	db "Hello, its me...", 0
	MenuName 	db "FirstMenu", 0

.data?
	hInstance	HINSTANCE ?
	hEdit 		HWND ?
	hButton 	HWND ?
	CommandLine 	LPSTR ?
.const
	EditID 		equ 	1
	ButtonID 	equ 	2

.code
start:
	invoke GetModuleHandle, NULL
	mov hInstance, eax
	invoke GetCommandLine
	mov CommandLine, eax
	invoke WinMain, hInstance, NULL, CommandLine, SW_SHOWDEFAULT
	invoke ExitProcess, eax
;
WinMain	proc hInst:HINSTANCE, hPrevInst:HINSTANCE, CmdLine:LPSTR, CmdShow:DWORD
	local wc:WNDCLASSEX
	local msg:MSG
	local hwnd:HWND

	mov	wc.cbSize, SIZEOF WNDCLASSEX
	mov	wc.style, CS_HREDRAW or CS_VREDRAW
	mov	wc.lpfnWndProc, offset WndProc
	mov	wc.cbClsExtra, NULL
	mov	wc.cbWndExtra, NULL
	push	hInst
	pop	wc.hInstance
	mov	wc.hbrBackground, COLOR_WINDOW+2
	mov	wc.lpszMenuName, offset MenuName
	mov	wc.lpszClassName, offset ClassName
	invoke	LoadIcon, NULL, IDI_APPLICATION
	mov	wc.hIcon, eax
	mov	wc.hIconSm, eax
	invoke	LoadCursor, NULL, IDC_ARROW
	mov	wc.hCursor, eax
	invoke	RegisterClassEx, addr wc
;
	invoke	CreateWindowEx, 0, addr ClassName, addr AppName,\
		WS_MAXIMIZE or WS_OVERLAPPEDWINDOW or WS_VISIBLE, CW_USEDEFAULT,\
	 	CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, NULL, NULL,\
		hInst, NULL
	mov	hwnd, eax
	invoke 	ShowWindow, hwnd, SW_SHOWMAXIMIZED
	invoke  UpdateWindow, hwnd
;
	.while TRUE
		invoke 	GetMessage, addr msg, NULL, 0, 0
		.break	.if (!eax)
		invoke	TranslateMessage, addr msg
		invoke	DispatchMessage, addr msg
	.endw
;
	mov eax, msg.wParam
	ret
WinMain	endp
;
WndProc proc hWnd:HWND, uMsg:UINT, wParam:WPARAM, lParam:LPARAM
	.if uMsg == WM_DESTROY
		invoke PostQuitMessage, 0 
	.elseif uMsg == WM_CREATE
		invoke CreateWindowEx, NULL, addr ButtonClassName,\
		  addr ButtonText, WS_CHILD or WS_VISIBLE or BS_DEFPUSHBUTTON,\
		  403, 280, 180,30, hWnd, ButtonID, hInstance, NULL
		mov hButton, eax
		invoke CreateWindowEx, WS_EX_CLIENTEDGE, addr EditClassName,\
		  NULL, WS_CHILD or WS_VISIBLE, 90,90,90,80, hWnd,\
		  EditID, hInstance, NULL
		mov hEdit, eax
	.elseif uMsg == WM_COMMAND
		mov eax, wParam
		.if ax == ButtonID
			invoke Beep, 1000, 1000
			shr eax, 16
			.if ax == BN_CLICKED
				invoke MessageBox, NULL, addr HelloWorld, addr HelloWorld, MB_OK
			.endif
		.endif
	.else
		invoke DefWindowProc, hWnd, uMsg, wParam, lParam
		ret
	.endif
	xor	eax, eax
	ret
WndProc endp
end 	start
