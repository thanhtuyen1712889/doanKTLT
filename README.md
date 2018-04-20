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
	wchar_t Sothich[10][256];
}; typedef struct SINHVIEN SV;

//Ham sao chep chuoi bo ky tu cuoi va dau

void DocSoThich(wchar_t *context, SV &sv, int &n)
{
	wchar_t a[2] = { '"','\0' };
	wchar_t *Sothich = NULL;
	n = 0;
	while (1)
	{
		n++;
		if (*context == '"')
		{
			Sothich = wcstok_s(context + 1, a, &context);
			
			if (*(context+1) == '"')
				context++;
		}
		else  
			Sothich = wcstok_s(context, L",", &context);
		if (Sothich == NULL)
		{
			n--;
			break;
		}
		wcscpy_s(sv.Sothich[n-1], Sothich);
	}
}

void Doc1SV(wchar_t *dong, SV &sv,int &soSothich)
{
	wchar_t *MSSV=NULL;
	wchar_t *Ten=NULL;
	wchar_t *Khoa = NULL;
	wchar_t *NienKhoa = NULL;
	wchar_t *Ngaysinh = NULL;
	wchar_t *Hinh = NULL;
	wchar_t *Mota = NULL;
	wchar_t a[2] = { '"','\0' };
	wchar_t *context = dong;
	MSSV = wcstok_s(context, L",", &context);
	Ten = wcstok_s(context, L",", &context);
	Khoa = wcstok_s(context, L",", &context);
	NienKhoa = wcstok_s(context, L",", &context);
	Ngaysinh = wcstok_s(context, L",", &context);
	Hinh = wcstok_s(context, L",", &context);
	if (context[0] == L'"')
	{
		Mota = wcstok_s(context + 1, a, &context);
		if (*(context + 1) == '"')
			context++;
	}
	else
	{
		Mota = wcstok_s(context, L",", &context);
	}
	wcscpy_s(sv.MSSV, MSSV);
	wcscpy_s(sv.Ten, Ten);
	wcscpy_s(sv.Khoa, Khoa);
	wcscpy_s(sv.NienKhoa, NienKhoa);
	wcscpy_s(sv.Ngaysinh, Ngaysinh);
	wcscpy_s(sv.Hinh, Hinh);
	wcscpy_s(sv.Mota, Mota);
	DocSoThich(context, sv, soSothich);
}

void DocAllFile(FILE *fp, SV * &sv, int &sosv,int * &soSothich)
{
	sosv = 0;
	sv = NULL;
	wchar_t dong[256];
	while (feof(fp) == 0)
	{
		sosv++;
		sv = (SV*)realloc(sv, sosv * sizeof(SV));
		soSothich = (int*)realloc(soSothich, sosv * sizeof(int));
		if (sv != NULL)
		{
			fgetws(dong, 255, fp);
			if (dong[wcslen(dong) - 1] == '\n')
				dong[wcslen(dong) - 1] = '\0';
			Doc1SV(dong, *(sv + sosv - 1),soSothich[sosv-1]);
		}
	}
}
void XuatSV(SV *sv, int sosv,int *soSothich)
{
	for (int i = 0; i < sosv; i++)
	{
		wprintf(L"\t-----Thông tin của sinh viên-----\n");
		wprintf(L"MSSV:     %s\n", sv[i].MSSV);
		wprintf(L"Họ và tên:    %s\n", sv[i].Ten);
		wprintf(L"Khoa:     %s\n", sv[i].Khoa);
		wprintf(L"Khóa học:   %s\n", sv[i].NienKhoa);
		wprintf(L"Ngày tháng năm sinh:   %s\n", sv[i].Ngaysinh);
		wprintf(L"Địa chỉ hình:   %s\n", sv[i].Hinh);
		wprintf(L"Mô tả bản thân:   %s\n", sv[i].Mota);
		wprintf(L"Sở thích: \n");
		for (int j = 0; j < soSothich[i]; j++)
		{
			wprintf(L"    %s\n", sv[i].Sothich[j]);
		}
		wprintf(L"\n\n");
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
/*
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
			case 4:
			{

				fseek(fp, -1L, SEEK_CUR);
				fputws(sv.Ten, fp);
				break;
			}
			case 1:
			case 5:
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
				fputws(sv.Sothich, fp);
				break;
			}
			//case 9:
			//{

			//fseek(fp, -1L, SEEK_CUR);
			//fputws(sv.Sothichdienanh, fp);
			//break;
			//}
			case 9:
			{
				fseek(fp, -1L, SEEK_CUR);
				fputws(sv.Mota, fp);
				return;
			}
			}
			k++;
			contro = ftell(fp);
			fseek(fp, contro + 1, SEEK_SET);
		}
	}
}
*/
int main()
{

	_setmode(_fileno(stdout), _O_U16TEXT);
	_setmode(_fileno(stdin), _O_U16TEXT);
	FILE *fp, *fpin, *fpout;
	SV *sv = NULL;
	wchar_t s[20];
	int sosv;
	int *soSothich = NULL;
	_wfopen_s(&fp, L"checklist1.txt", L"r,ccs=UTF-8");
	fopen_s(&fpin, "sinhvien.htm", "r,ccs=UTF-8");

	if (!fp || !fpin)
		printf("Khong mo duoc tap tin\n");
	else
	{
		DocAllFile(fp, sv, sosv,soSothich);
		//wprintf(L"%s", sv[1].Sothich[0]);
		XuatSV(sv, sosv,soSothich);
		wprintf(L"%d", soSothich[0]);
	}
	/*for (int i = 0; i < sosv; i++)
	{
		wcscpy_s(s, sv[i].MSSV);
		wcscat_s(s, L".htm");
		_wfopen_s(&fpout, s, L"w+,ccs=UTF-8");
		if (fpout != NULL)
		{
			SaoChepFile(fpin, fpout);
			rewind(fpout);
			//SuaFile(fpout, sv[i]);
			fclose(fpout);
			rewind(fpin);
		}
		else printf("Khong mo duoc file %s", s);
	}

	fclose(fp);
	fclose(fpin);
	*/
	return 0;

}
