#include <stdio.h>
#include <malloc.h>
#include <string.h>
#include <conio.h>
struct SINHVIEN
{
	char MSSV[11];
	char Ten[31];
	char Khoa[31];
	char NienKhoa[5];
	char Ngaysinh[11];
	char Hinh[20];
	char Mota[1000];
	char Sothichnhac[200];
	char Sothichdienanh[200];
}; typedef struct SINHVIEN SV;
//Ham sao chep chuoi bo ky tu cuoi va dau
char* StringCopy(char* dich, char* nguon)
{
	int n = strlen(nguon);
	for (int i = 1; i < n; i++)
	{
		dich[i-1] = nguon[i];
	}
	dich[n-2] = '\0';
	return dich;
}
void Doc1SV(char *dong, SV &sv)
{
	int k = 0;
	int n=strlen(dong);
	for (int i = n-1; i > 0; i--)
	{
		if (dong[i] == ','&& dong[i-1]=='"')
		{
			switch (k)
			{
			case 0:
				StringCopy(sv.Sothichdienanh, dong + i+1);
				break;
			case 1:
				StringCopy(sv.Sothichnhac, dong + i +1);
				break;
			case 2: 
				StringCopy(sv.Mota, dong + i +1);
				break;
			case 3:
				StringCopy(sv.Hinh, dong + i +1);
				break;
			case 4:
				StringCopy(sv.Ngaysinh, dong + i +1);
				break;
			case 5:
				StringCopy(sv.NienKhoa, dong + i +1);
				break;
			case 6:
				StringCopy(sv.Khoa, dong + i +1);
				break;
			case 7:
				StringCopy(sv.Ten, dong + i +1);
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
	char dong[256];
	sosv = 0;
	sv = NULL;
	while (feof(fp) == 0)
	{
		sosv++;
		sv = (SV*)realloc(sv, sosv * sizeof(SV));
		if (sv != NULL)
		{
			fgets(dong, 255, fp);
			if (strlen(dong) < 255)
				dong[strlen(dong)] = '\0';
			Doc1SV(dong, sv[sosv - 1]);
		}
	}
}
void XuatSV(SV *sv,int sosv)
{
	for (int i = 0; i < sosv; i++)
	{
		printf("%s %s %s %s %s %s %s %s %s\n", &sv[i].MSSV, &sv[i].Ten, &sv[i].Khoa, &sv[i].NienKhoa, &sv[i].Ngaysinh, &sv[i].Hinh, &sv[i].Mota, &sv[i].Sothichnhac, &sv[i].Sothichdienanh);
	}
}
void main()
{
	FILE *fp;
	SV *sv = NULL;
	int sosv=0;
	fopen_s(&fp,"sinhvien.txt", "r");
	if (fp==NULL)
		printf("Khong mo duoc tap tin\n");
	else
	{
		/*
		SV a;
		char dong[255];
		fgets(dong, 255, fp);
		if (strlen(dong) < 255)
			dong[strlen(dong)-1] = '\0';
		Doc1SV(dong, a);
		printf("%s\n",a.MSSV);
		*/
		DocAllFile(fp, sv, sosv);
		XuatSV( sv, sosv);
		fclose(fp);

	}
}
