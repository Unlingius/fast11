// тестируем метод особых точек FAST в зависимости от порога t, значения n
void FASTfeatureTest(uchar * im, uchar * imres, int w, int h, int R, int n, int t)
{
	// массивы результатов сравнений центральной точки и окружностей радиуса 1 2 3
	int mass1[8]; int mass2[12]; int mass3[16];  // 0 - темнее, 1 - похожий, 2 - светлее
	
	// смещения от центральной точки к точкам окружности
	int di1[8];
	di1[0]=-1*w; di1[1]=-1*w+1; di1[2]=0*w+1; di1[3]=1*w+1; di1[4]=1*w; di1[5]=1*w-1; di1[6]=0*w-1; di1[7]=-1*w-1;
	int di2[12];
	di2[0]=-2*w; di2[1]=-2*w+1; di2[2]=-1*w+2; di2[3]=0*w+2; di2[4]=1*w+2; di2[5]=2*w+1; di2[6]=2*w; di2[7]=2*w-1;di2[8]=1*w-2;di2[9]=0*w-2;di2[10]=-1*w-2;di2[11]=-2*w-1;
	int di3[16];
	di3[0]= - 3*w;di3[1]= - 3*w+1;	di3[2]= - 2*w+2;di3[3]= - 1*w+3;di3[4]= - 0*w+3;di3[5]= 1*w+3;di3[6]= 2*w+2;di3[7]= 3*w+1;
	di3[8]= 3*w+0;di3[9]= 3*w-1;di3[10]= 2*w-2;di3[11]= 1*w-3;di3[12]= 0*w-3;di3[13]= - 1*w-3;di3[14]= - 2*w-2;di3[15]= - 3*w-1;

	// проходимся по всем точкам изображения
	for (int y = 4; y < h - 4; y++) for (int x = 4; x < w - 4; x++)
	{
		// номер центральной точки
		int i0 = y*w + x; 
		int cont0 = 0;int cont2 = 0;// число непрерывно идущих пикселов темнее и светлее соответственно
		bool isfeature = false;  // особая точка или нет

		// круг радиуса 1
		if (R==1)
		{
			for (int m=0; m<8; m++)
			{
				if (im[i0+di1[m]] > im[i0]+t) mass1[m]=2;
				else if (im[i0+di1[m]] < im[i0]-t) mass1[m]=0;
				else mass1[m]=1;
			}
			cont0=0; cont2=0; 
			for (int i =0; i<8; i++)
			{
				if (mass1[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass1[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
			for (int i =0; i<8; i++)
			{
				if (mass1[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass1[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
		}

		// радиус 2
		if (R==2)
		{
			// вычисляем соотношения для всех точек круга
			for (int m=0; m<12; m++)
			{
				if (im[i0+di2[m]] > im[i0]+t) mass2[m]=2;
				else if (im[i0+di2[m]] < im[i0]-t) mass2[m]=0;
				else mass2[m]=1;
			}
			// анализируем и выносим решение
			cont0=0; cont2=0; 
			for (int i =0; i<12; i++)
			{
				if (mass2[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass2[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
			for (int i =0; i<12; i++)
			{
				if (mass2[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass2[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
		}

		if (R==3)
		{
			for (int m=0; m<16; m++)
			{
				if (im[i0+di3[m]] > im[i0]+t) mass3[m]=2;
				else if (im[i0+di3[m]] < im[i0]-t) mass3[m]=0;
				else mass3[m]=1;
			}
			cont0=0; cont2=0;
			for (int i =0; i<16; i++)
			{
				if (mass3[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass3[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
			for (int i =0; i<16; i++)
			{
				if (mass3[i]==0) cont0++; else cont0=0; if (cont0==n) {isfeature=true; break;}
				if (mass3[i]==2) cont2++; else cont2=0; if (cont2==n) {isfeature=true; break;}
			}
		}

		// закрашиваем особые
		if (isfeature) imres[i0]=255;
		else imres[i0]=im[i0]*0.5;		
	}

}

void SaveBMP(char * filename, uchar * ptr, int width, int height)
{
	FILE * vstream = fopen(filename,"wb");			
	unsigned short FileType = 19778;
	unsigned long  FileSize;
	unsigned short Reserv1  =     0;
	unsigned short Reserv2  =     0;
	unsigned long  Offset   =  54;
	unsigned long  InfSize  =    40;
	unsigned long  BmWidht = width;
	unsigned long  BmHight = height;
	unsigned short Planes   =     1;
	unsigned short Bits     =     24;
	unsigned long  Compress =     0 ;
	unsigned long  SizeImage;
	unsigned long  XperMetr =     0;
	unsigned long  YperMetr =     0;
	unsigned long  UsColors =   256;
	unsigned long  ImpColors=     0;
	// запись шапки выходного файла
	SizeImage = width*height*3;
	FileSize = SizeImage+Offset;
	fseek(vstream,0,SEEK_SET);
	fwrite(&FileType, 2,1,vstream);fwrite(&FileSize, 4,1,vstream);
	fwrite(&Reserv1 , 2,1,vstream);fwrite(&Reserv2 , 2,1,vstream);
	fwrite(&Offset  , 4,1,vstream);fwrite(&InfSize,  4,1,vstream);
	fwrite(&width,  4,1,vstream);fwrite(&height,  4,1,vstream);
	fwrite(&Planes ,  2,1,vstream);fwrite(&Bits   ,  2,1,vstream);
	fwrite(&Compress, 4,1,vstream);fwrite(&SizeImage,4,1,vstream);
	fwrite(&XperMetr, 4,1,vstream);fwrite(&YperMetr, 4,1,vstream);
	fwrite(&UsColors, 4,1,vstream);fwrite(&ImpColors,4,1,vstream);

	// запись файла в массив по строкам
	for (int j=0; j<height; j++)
	{
		double Seek_Position = Offset + 3*(height-j)*width;
		fseek(vstream,Seek_Position,SEEK_SET);
		fwrite(ptr+j*3*width, 1, width*3, vstream);
	}
	fclose(vstream);
}

uchar * LoadBMP(char * filename, int * width, int * height)
{
	// считываем шапку
	FILE * istream = fopen(filename,"rb");
	unsigned short FileType = 19778;
	unsigned long  FileSize;
	unsigned short Reserv1  =     0;
	unsigned short Reserv2  =     0;
	unsigned long  Offset   =  1078;
	unsigned long  InfSize  =    40;
	unsigned long  BmWidht;
	unsigned long  BmHight;
	unsigned short Planes   =     1;
	unsigned short Bits     =     8;
	unsigned long  Compress =     0 ;
	unsigned long  SizeImage;
	unsigned long  XperMetr =     0;
	unsigned long  YperMetr =     0;
	unsigned long  UsColors =   256;
	unsigned long  ImpColors=     0;
	fseek(istream,0,SEEK_SET);
	fread(&FileType, 2,1,istream);fread(&FileSize, 4,1,istream);
	fread(&Reserv1 , 2,1,istream);fread(&Reserv2 , 2,1,istream);
	fread(&Offset  , 4,1,istream);fread(&InfSize,  4,1,istream);
	fread(&BmWidht,  4,1,istream);fread(&BmHight,  4,1,istream);
	fread(&Planes ,  2,1,istream);fread(&Bits   ,  2,1,istream);
	fread(&Compress, 4,1,istream);fread(&SizeImage,4,1,istream);
	fread(&XperMetr, 4,1,istream);fread(&YperMetr, 4,1,istream);
	fread(&UsColors, 4,1,istream);fread(&ImpColors,4,1,istream);

	// создаём массив
	uchar * ptr = new uchar[3*BmWidht*BmHight];
	
	// запись файла в массив по строкам
	for (int j=0; j<BmHight; j++)
	{
		double Seek_Position = Offset + 3*(BmHight-j)*BmWidht;
		fseek(istream,Seek_Position,SEEK_SET);
		fread(ptr+j*3*BmWidht, 1, BmWidht*3, istream);
	}

	*width = BmWidht;
	*height = BmHight;
	return ptr;
}


// переходы между 1 и 3 канальными картинками
void im1to3(uchar * im1, uchar * im3, int size1)
{
	int i1=0; int i3=0;
	for (int i1=0; i1<size1; i1++){ im3[i3]=im1[i1];im3[i3+1]=im1[i1];im3[i3+2]=im1[i1];i3+=3;}
}
void im3to1(uchar * im3, uchar * im1, int size1)
{
	int i1=0; int i3=0;
	for (int i1=0; i1<size1; i1++){ im1[i1]=(im3[i3]+im3[i3+1]+im3[i3+2])/3; i3+=3;}
}


int main()
{
	int w = 0;int h = 0;

	// загрузим картинку
	uchar * ptr = LoadBMP("in.bmp", &w,&h);
	uchar * ptr1 = new uchar[w*h];
	uchar * ptr2 = new uchar[w*h];
	uchar * ptr3 = new uchar[3*w*h];
	
	// в градации серого
	im3to1(ptr, ptr1, w*h);

	// обрабатываем детектором фаст
	// параметры, 
	FASTfeatureTest(ptr1, ptr2, w, h, 3, 9, 50);

	// обратно в цветной
	im1to3(ptr2,ptr3,w*h);

	// сохраним
	SaveBMP("out.bmp", ptr3, w, h);
	return 0;
}
