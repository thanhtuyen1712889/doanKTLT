#include <stdio.h>
struct SINHVIEN
{
	char MSSV[11];
	char Ten[31];
	char Khoa[31];
	int Namhoc;
	char ntnsinh[11];
	char hinh[20];
	char mota[1000];
	char sothich[300];
}; typedef struct SINHVIEN SV;
void DocFile(FILE *fp, SV* sv, int sosv)
{
	char *temp;
	sv = NULL;
	sosv = 0;
	while (feof(fp) != 0)
	{
		sosv++;
		sv = (SV*)realloc(sv, sosv * sizeof(SV));
		char temp[256];
		fgets(temp, 255, fp);
		if(strlen(temp)<255)
			temp[strlen(temp) - 1] = '\0';

	}
}
void XuatSV(SV *sv,int sosv)
{
	for (int i = 0; i < sosv; i++)
	{
		printf("%s,%s,%s,%d,%s,%s,%s,%s%s", &sv[i].MSSV, &sv[i].Ten, &sv[i].Khoa, &sv[i].NienKhoa, &sv[i].ntnsinh, &sv[i].hinh, &sv[i].mota, &sv[i].sothichnhac, &sv[i].sothichdienanh);
	}
}
void main()
{
	FILE *fp;
	SV *sv = NULL;
	int sosv;
	fopen_s(&fp,"sinhvien.txt", "r");
	if (fp==NULL)
		printf("Khong mo duoc tap tin\n");
	else
	{
		//XuatSV(sv, sosv);
		//fclose(fp);
	}
}
