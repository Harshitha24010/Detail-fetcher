With the details of proctees listed in a text file, implement a program to do the following:
1. Categorize Day Scholars and Hostelites:
○ Parse the file and create separate groups for Day Scholars and Hostelites using appropriate data structures.
2. List Contact Details of a Particular Student:
○ Accept the student’s name or ID as input and display their contact details (name, contact number, address, category). Handle cases where the student is not found.
3. List Proctees in Alphabetical Order of Their Names:
○ Sort the data based on student names and display the sorted list with all details.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure to hold student details
struct student
{
    char name[500];
    char id[500];
    char contact_number[500];
    char address[500];
    char category[500];  // "Day Scholar" or "Hostelite"
};

struct student Student[500];
struct student day_scholars[500];
struct student hostelites[500];
int day_scholars_count = 0, hostelites_count = 0;
int line = 0;

// Function prototypes
void read_students_from_file();
void sort_students();
void display_student_details() ;

void main() 
{
    
    int choice;
    for(;;)
    {
    printf("\nMenu:\n");
    printf("1. Group and display students as Day Scholars and Hostelites\n");
    printf("2.Display the details of a Student\n");
    printf("3.List the Proctee in sorted order\n");
    printf("4. Exit\n");

    
    {
        printf("Enter your choice:\n ");
        scanf("%d", &choice);
        switch (choice) 
        {
            case 1:
                read_students_from_file();
                break;
    
            case 2: display_student_details();
                    break;
            case 3: sort_students();
                    break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}
}
// Function to read data from a file and categorize students
void read_students_from_file()
{
    FILE *fp;
    fp = fopen("C:/Users/HP/Desktop/students.txt","r");

    if (fp == NULL) {
        printf("Error opening file.\n");
        return;
    }
    int i = 0;
    while (fscanf(fp, "%70[^,],%70[^,],%70[^,],%70[^,],%50[^\n]\n",
                  Student[i].id, Student[i].name, Student[i].contact_number, 
                  Student[i].address, Student[i].category) == 5)
    {
        if (strstr(Student[i].category, "Day Scholar") != NULL)
        {
            day_scholars[day_scholars_count++] = Student[i];
        }
        else if (strstr(Student[i].category, "Hostelite") != NULL) 
        {
            hostelites[hostelites_count++] = Student[i];
        }
        i++;
    }
    printf("Number of Day scholars : %d\n",day_scholars_count);
    printf("Number of Hostelites : %d\n",hostelites_count);
    line = i;
    fclose(fp);
}

// Function to sort students alphabetically by name
void sort_students()
{
    struct student temp;
    // Sort Day Scholars
    for (int i = 0; i < line - 1; i++) {
        for (int j = i + 1; j < line; j++) {
            if (strcmp(Student[i].name, Student[j].name) > 0) {
                temp = Student[i];
                Student[i] = Student[j];
                Student[j] = temp;
            }
        }
    }

    printf("\n Students list in Sorted order\n");
    for (int i = 0; i < line; i++) {
        printf("Name: %s, ID: %s, Contact: %s, Address: %s,Category: %s\n",
               Student[i].name, Student[i].id, Student[i].contact_number, 
               Student[i].address,Student[i].category);
    }
}
void display_student_details() 
{
    char identifier[50];
    printf("Enter the student ID\n");
    scanf("%s", identifier);

    for (int i = 0; i < line; i++) {
        if (strstr(Student[i].id, identifier) != NULL) {
            printf("ID: %s\nName: %s\nContact Number: %s\nAddress: %s\nCategory: %s\n",
                   Student[i].id, Student[i].name, Student[i].contact_number,
                   Student[i].address, Student[i].category);
            return;
        }
    }

    printf("Student with ID %s not found.\n",identifier);
}
