README.md
<br>
Библиотека для создания читов на платформе .NET 4
<b>ВНИМАНИЕ! ЧИТ БУДЕТ РАБОТАТЬ, ЕСЛИ ОН СКОМПИЛИРОВАН ДЛЯ x86 АРХИТЕКТУРЫ (32 бита)</b>
<br>
<br>
Использование:<br>
Для начала нам нужно написать using Nummer.Cheats;<br>
<br>
1) Конвертирование сигнатуры (строка) в массив байтов.<br>
Использование:<br>
Tools.StringToByteArray("10 6B 28 FF"); - возвращает new byte[] {0x10, 0x6B, 0x28, 0xFF};<br>
 ВНИМАНИЕ! ФУНКЦИЯ НЕ УМЕЕТ ЧИТАТЬ МАСКУ СИГНАТУРЫ!

2) Поиск байт в памяти:<br>
Использование:<br>
new AOBScan(ProcessID).AobScan(byte[]);<br>
 функция вернёт адрес найденой сигнатуры.<br>
Адрес будет в типе IntPtr.<br>
ProcessID - это ID процесса в котором надо найти сигнатуру.<br>
AobScan(byte[]) принимает только массив байт для поиска (БЕЗ МАСКИ)

3) Поиск сигнатуры с маской:<br>
SigScan _sigScan = new SigScan(someProc, new IntPtr(0x123456), 0x1000);<br>
 IntPtr pAddr = _sigScan.FindPattern(new byte[]{ 0xFF, 0xFF, 0xFF, 0xFF, 0x51, 0x55, 0xFC, 0x11 }, "xxxx?xx?", 12);<br>
 someProc - это процесс в котором надо искать.<br>
Тип System.Diagnostics.Process .<br>
Далее идёт начальный адрес (с которого сканировать), и размер для дампа сканирования.<br>
FindPattern принимает массив байтов для поиска, потом маску типа String , и смещение от найденного адреса.<br>
Если смещение не нужно, то нужно просто передать 0.

4) Запись, и чтение из адресов:<br>
var proc = new ProcessMemory((uint)PIDPROCESS);<br>
 proc.StartProcess();<br>
 proc.WriteMem(SomeAdress, bytes);<br>
 Для начала нам нужно открыть процесс, и это мы выполняем в первой строке.<br>
PIDPROCESS - это ID процесса в котором надо выполнить замену , или чтение байт.<br>
Далее мы открываем процесс для записи, и чтения.<br>
Потом мы записываем по адресу (int) байты (byte) .<br>
Для чтения, и записи других типов есть такие функции:<br>
DllImageAddress(string dllname) - возвращает адрес модуля DLL в процессе.<br>
ImageAddress(int pOffset) - возвращает начальный адрес процесса со смещением pOffset MyProcessName() - возвращает название процесса в типе String Pointer((bool)(string), int, int....) - возвращает указатель.<br>
Принимает переменную типа bool , если нужно вернуть указатель от главного модуля процесса, или строку с названием модуля, если нужно вернуть указатель от модуля.<br>
И принимает до 6 смещений.<br>
ReadByte((bool)(string), int) - возвращает байт, который был считан с модуля, или процесса (так же, как и в указателях) по адресу, который передаётся в типе int Подобные функции ReadByte:<br>
ReadFloat(), ReadInt(), ReadShort(), ReadUInt(), ReadDouble().<br>
ReadMem(int,int,(bool)) - читает массив байтов из памяти.<br>
Первый параметр - это смещение откуда читать (адрес) , второй - это размер байтов которые надо считать, последний параметр типа bool использовать только, если нужно читать байты с базового модуля процесса.<br>
ReadStringAscii((bool)(string), int, int) - первые параметры понятно из прошлых функций , воторой параметр - это смещение, а 3 параметр - это размер.<br>
ReadStringUnicode() - подобная ReadStringAscii.<br>
WriteByte((bool)(string), int, byte) - записывает байт.<br>
второй параметр - смещение, а 3 это байт который надо записать.<br>
Подобные функции WriteByte:<br>
WriteDouble(), WriteFloat(), WriteInt(), WriteShort(), WriteStringAscii(), WriteStringUnicode(), WriteUInt().<br>
WriteMem(int, byte[], (bool)) - первый парамерт - это смещение.<br>
Второй - это байты которые надо записать.<br>
3 параметр - это писать ли в базовый адрес у процесса (не обязателен)

Version 1.0 .<br>
Все права защищены (c) Nummer.