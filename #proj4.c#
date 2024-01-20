#include <stdio.h>
#include <sys/stat.h>
#include <sys/time.h>
#include <stdlib.h>

/*
   Purpose: Reads input 1 and 2 and adds any discrepencies from file 1,
            into a new file called differencesFoundInFile1.txt
   Inputs: File 1 pointer and File 2 pointer
   Assumption: File 1 and 2 must not be NULL
 */
void step1(FILE * fp1 , FILE * fp2);
/*
   Purpose: Reads file 1 and 2 into dynamic arrays. The discrepencies between these arrays
            are read into a file called differencesFoundInFile2.
   Inputs: File 1 pointer and File 2 pointer
   Assumption: File 1 and 2 must not be NULL
 */
void step2(FILE * fp1 , FILE * fp2);
/*
   Purpose: Records the amount of time it takes for a functin
            that takes in two files to execute.
   Inputs: File 1 pointer and File 2 pointer, function f to time
   Outputs: The amount of time it takes to excecute.
*/
double timer(FILE * fp1, FILE * fp2, void (*f)(FILE * fp1, FILE * fp2));
/*
   Purpose: Returns the amount of charecters in a file
   Inputs: The file to evaluate
   Outputs: The time it has taken
 */
int getSize(FILE * f);
/*
   Purpose: Converts a file to a dynamic array of charecters
   Inputs: The file to convert
   Outputs: The charecter array
 */
char * getFileArray(FILE * f);
/*
   Purpose: Executes step 1 and 2 and times them in milliseconds
   Inputs: Command line args
   Outputs: Success status
 */
int main (int argc, char * argv[]) {
    if (argc != 3) { //Usage statment
        printf("Usage: proj4.out <file1> <file2>\n");
        return 1;
    }
    char * fName1 = argv[1]; //"input1.txt"
    char * fName2 = argv[2]; //"input2.txt"
    FILE *fp1 = fopen(fName1, "r"); //open for reading
    FILE *fp2 = fopen(fName2, "r"); //returns a file pointer
    if (fp1 == NULL || fp2 == NULL) {
        printf("There was an error reading a file.\n");
        return 1;
    }
    void (*step1_ptr)(FILE * fp1, FILE * fp2) = &step1; //pointer to step 1
    printf("Step 1 took %f milliseconds\n",timer(fp1,fp2,step1_ptr)); //Time step 1
    void (*step2_ptr)(FILE * fp1, FILE * fp2) = &step2; //pointer to step 2
    printf("Step 2 took %f milliseconds\n",timer(fp1,fp2,step2_ptr)); //Time step 2
    fclose(fp1);fclose(fp2);
    return 0; //SUCCESS!
}

void step1( FILE * fp1, FILE * fp2) {
    FILE * diff1 = fopen("differencesFoundInFile1.txt", "w"); //writing
    if (diff1 == NULL) {
        printf("There was an error writing to a file.");
        exit(1);
    }
    char ch1; char ch2;
    while (ch1 != EOF) {
        ch1 = fgetc(fp1); ch2 = fgetc(fp2); //Read chars from file
        if (ch1 != ch2 && ch1 != -1) {
            putc(ch1,diff1); //assign to difference one file
        }
    }
    fclose(diff1);
}

void step2(FILE * fp1, FILE * fp2) {
    char * diffArr = malloc(getSize(fp2)); //difference array
    char * diffP = diffArr; //Array pointers
    FILE * diff2 = fopen("differencesFoundInFile2.txt", "w"); //difference2 file open
    if (diff2 == NULL) {
        printf("Error opening file");
    }
    char * array1 = getFileArray(fp1);
    char * array2 = getFileArray(fp2);
    int count = 0;
    for (int i = 0 ; i < getSize(fp2) ; i++) { //get all differences
        if (*(array1+i) != *(array2+i)) {
            *diffP = *(array2+i); //store in array
            diffP++; count++;
        }
    }
    for (int i = 0; i < count ; i++) {
        fputc(*(diffArr+i), diff2);
    }
    free(array1) ; free(array2); free(diffArr);
    fclose(diff2);
}

char * getFileArray(FILE * f) {
    char * array = malloc(sizeof(char)*getSize(f));
    char ch; char * p = array;
    while((ch=fgetc(f)) != EOF) {
        *p = ch;
        p++;
    }
    return array;
}

int getSize(FILE * f) {
    fseek(f, 0, SEEK_END);
    int size = ftell(f);
    fseek(f, 0, SEEK_SET);
    return size;
}

double timer(FILE * fp1, FILE * fp2, void (*f)(FILE * fp1, FILE * fp2)) {
    struct timeval before, after;
    gettimeofday(&before,NULL);
    f(fp1,fp2);
    gettimeofday(&after,NULL);
    double interval = after.tv_usec - before.tv_usec;
    if (interval < 0) {
        interval *= -1;
    }
    return interval/1000;
}
