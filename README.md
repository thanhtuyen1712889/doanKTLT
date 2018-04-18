#include <stdio.h>
#include <malloc.h>
#include <string.h>
#include <conio.h>
#include <fcntl.h>
#include <io.h>
#include <wchar.h>


struct SINHVIEN
{
wchar_t MSSV[11];
wchar_t Ten[31];
wchar_t Khoa[31];
wchar_t NienKhoa[5];
wchar_t Ngaysinh[11];
wchar_t Hinh[20];
wchar_t Mota[1000];
wchar_t Sothichnhac[70];
wchar_t Sothichdienanh[70];
}; typedef struct SINHVIEN SV;

//Ham sao chep chuoi bo ky tu cuoi va dau

wchar_t* StringCopy(wchar_t* dich, wchar_t* nguon)
{

int n = wcslen(nguon);
for (int i = 1; i < n; i++)
{
dich[i - 1] = nguon[i];
}
dich[n - 2] = '\0';
return dich;
}
void Doc1SV(wchar_t *dong, SV &sv)
{
int k = 0;
int n = wcslen(dong);
for (int i = n - 1; i > 0; i--)
{
if (dong[i] == ','&& dong[i - 1] == '"')
{
switch (k)
{
case 0:
StringCopy(sv.Sothichdienanh, dong + i + 1);
break;
case 1:
StringCopy(sv.Sothichnhac, dong + i + 1);
break;
case 2:
StringCopy(sv.Mota, dong + i + 1);
break;
case 3:
StringCopy(sv.Hinh, dong + i + 1);
break;
case 4:
StringCopy(sv.Ngaysinh, dong + i + 1);
break;
case 5:
StringCopy(sv.NienKhoa, dong + i + 1);
break;
case 6:
StringCopy(sv.Khoa, dong + i + 1);
break;
case 7:
StringCopy(sv.Ten, dong + i + 1);
break;
}
dong[i] = '\0';
k++;
}
if (k == 8)
{
i = 0;
StringCopy(sv.MSSV, dong + i);
}
}
}

void DocAllFile(FILE *fp, SV * &sv, int &sosv)
{
wchar_t dong[256];
sosv = 0;
sv = NULL;
while (feof(fp) == 0)
{
sosv++;
sv = (SV*)realloc(sv, sosv * sizeof(SV));
if (sv != NULL)
{
fgetws(dong, 255, fp);
if (dong[wcslen(dong)-1] =='\n')
dong[wcslen(dong)-1] = '\0';
Doc1SV(dong, sv[sosv - 1]);
}
}
}
void XuatSV(SV *sv, int sosv)
{
for (int i = 0; i < sosv; i++)
{
wprintf(L"%s %s %s %s %s %s %s %s %s\n", &sv[i].MSSV, &sv[i].Ten, &sv[i].Khoa, &sv[i].NienKhoa, &sv[i].Ngaysinh, &sv[i].Hinh, &sv[i].Mota, &sv[i].Sothichnhac, &sv[i].Sothichdienanh);
}
}
void SaoChepFile(FILE *filenguon, FILE *filexuat)
{
wchar_t temp[256];
while (!feof(filenguon))
{
fgetws(temp, 255, filenguon);
fputws(temp, filexuat);
}
}
void SuaFile(FILE *&fp, SV sv)
{
long contro;
int k = 0;
wchar_t ch;
while (!feof(fp))
{
	fwscanf_s(fp, L"%c", &ch);
if (ch == L'*')
{
switch (k)
{
case 0:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Ten, fp);
break;
}
case 1:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.MSSV, fp);
break;
}
case 2:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Khoa, fp);
break;
}
case 3:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Hinh, fp);
break;
}
case 4:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Ten, fp);
break;
}
case 5:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.MSSV, fp);
break;
}
case 6:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Khoa, fp);
break;
}
case 7:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Ngaysinh, fp);
break;
}
case 8:
{

fseek(fp, -1L, SEEK_CUR);
fputws(sv.Sothichnhac, fp);
break;
}
case 9:
{

fseek(fp, -1L, SEEK_CUR);
fputws(sv.Sothichdienanh, fp);
break;
}
case 10:
{
fseek(fp, -1L, SEEK_CUR);
fputws(sv.Mota, fp);
return;
}
}
k++;
contro = ftell(fp);
fseek(fp, contro+1, SEEK_SET);
}
}
}

int main()
{

	_setmode(_fileno(stdout), _O_U16TEXT);
	_setmode(_fileno(stdin), _O_U16TEXT);
	FILE *fp, *fpin,*fpout;
	SV *sv = NULL;
	wchar_t s[20];
	int sosv;
	_wfopen_s(&fp, L"checklist.txt", L"r,ccs=UTF-8");
	fopen_s(&fpin, "sinhvien.htm", "r,ccs=UTF-8");
	if (!fp || !fpin)
	printf("Khong mo duoc tap tin\n");
	else
	{

		DocAllFile(fp, sv, sosv);
		XuatSV(sv, sosv);
	
	for (int i = 0; i < sosv; i++)
	{
		wcscpy_s(s, sv[i].MSSV);
		wcscat_s(s, L".htm");
	_wfopen_s(&fpout, s, L"w+,ccs=UTF-8");
	if (fpout!=NULL)
	{
	SaoChepFile(fpin, fpout);
	rewind(fpout);
	SuaFile(fpout, sv[i]);
	fclose(fpout);
	rewind(fpin);
	}
	else printf("Khong mo duoc file %s", s);
	}
	
	fclose(fp);
	fclose(fpin);
	} 
	
	
	
}
