# MD5-Collision-Attack-Lab

#2.1 Task 1: Generating Two Different Files with the Same MD5 Hash

  -p PREFIX_FILE -o OUTPUTFILE1 OUTPUTFILE2
  
To test this out, I created a file hi.txt and truncated it using truncate   -s YOUR_DESIRED_SIZE hi.txt. After running md5collgen -p hi.txt -o hi1 hi2 looking at the results using bless hi1, we can see that it has been padded with zeros. This is because MD5 processes blocks of size 64 bytes.

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/1.png)

#2.2 Task 2: Understanding MD5’s Property

To test this create a file hi.txt and run md5collgen -p hi.txt -o hi1 hi2

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/2.png)

Verify that the MD5 hashes are the same using md5sum hi1 hi2.

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/3.png)

Now let’s append a random string to the end of both files and check the MD5 hashes of both files again.

    echo hi >> hi1
    echo hi >> hi2
    md5sum hi1 hi2

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/4.png)

We can see that the MD5 hashes remain identical.

#2.3 Task 3: Generating Two Executable Files with the Same MD5 Hash

Save the following code in a file program.c.


#include <stdio.h>
unsigned char a[200] = { 'H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A'};
int main()
{
        int i;
        a[199]='D';
        a[198]='N';
        a[197]='E';
        for(i = 0; i < 200; i++)
        {
                printf("%x", a[i]);
        }
        printf("\n");
}



Running head -c 4224 a.out > prefix and md5collgen -p prefix -o agen bgen ,

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/5.png)

Now we have two files with the same MD5 hash but different suffixes. Looking at both agen and bgen in bless ( bless agen bgen ).

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/6.png)
A part of agen
![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/7.png)
A portion of bgen

Now we will get the common end to be appended using tail -c 4353 a.out > commonend.

Put them together by running,

      cat commonend >> agen
      cat commonend >> bgen


Add executable permission to both files and run them. Note that the output differs.

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/8.png)


#2.4 Task 4: Making the Two Programs Behave Differently

This is the program we will be using. Copy the contents and save it as program.c.


#include <stdio.h>
unsigned char a[300] = { 'H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A'};
unsigned char b[300] = { 'H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A','H','A','A','A','A','A','A','A','A','A'};
int main()
{
        int i;
        int isSame=1;
        for(i = 0; i < 200; i++)
        {
                if(a[i]!=b[i])
                        isSame=0;
        }
        if(isSame)
                printf("This would run the safe code and display the intended behaviour");
        else
                printf("This is where malicious code would be run");
        printf("\n");
}



Now we compile the program using gcc program.c. The output binary is stored by default as a.out. Looking at the contents of a.out using bless a.out.

![alt text](https://github.com/shoaibqureshi6/MD5-Collision-Attack-Lab/blob/main/9.png)

To create the common end file, we would need the characters from the end file created earlier after accounting for a 128+192 (size of middle) offset. Do this by running tail -c +321 end > commonend .

Now to put all the files together. Run

      cp pgen benigncode
      cp qgen maliciouscode
      cat middle >> benigncode
      cat middle >> maliciouscode
      cat p >> benigncode
      cat p >> maliciouscode
      cat commonend >> benigncode
      cat commonend >> maliciouscode

Verify using md5sum benigncode maliciouscode and check if the hashes are the same. Also, make sure the size of a.out, benigncode and maliciouscode are the same.

Finally, run

      chmod +x benigncode maliciouscode
      ./benigncode
      ./maliciouscode
