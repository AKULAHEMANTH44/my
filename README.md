#include <stdio.h>
#include <string.h>

#define MAX_JOBS 10
#define MAX_APPLICATIONS 50

typedef struct {
    int id;
    char company[80];
    char role[80];
    char skills[150];
    char location[80];
} Job;

typedef struct {
    char studentName[80];
    char email[100];
    int jobId;
} Application;

Job jobs[MAX_JOBS] = {
    {1, "TechNova Solutions", "Embedded Systems Intern",
     "C, Microcontrollers, Embedded Systems", "Bangalore"},

    {2, "CircuitSoft Technologies", "Firmware Developer",
     "C, RTOS, ARM, Electronics", "Chennai"},

    {3, "DataWave Systems", "Junior Software Developer",
     "C++, Data Structures, Problem Solving", "Hyderabad"},

    {4, "RoboWorks India", "Robotics Intern",
     "Arduino, C, Sensors, Robotics", "Pune"}
};

Application applications[MAX_APPLICATIONS];
int applicationCount = 0;
int jobCount = 4;

void showJobs() {
    printf("\n========== Available Opportunities ==========\n");

    for (int i = 0; i < jobCount; i++) {
        printf("\nJob ID: %d\n", jobs[i].id);
        printf("Company : %s\n", jobs[i].company);
        printf("Role    : %s\n", jobs[i].role);
        printf("Skills  : %s\n", jobs[i].skills);
        printf("Location: %s\n", jobs[i].location);
    }

    printf("\n=============================================\n");
}

void applyForJob() {
    int jobId;
    int found = 0;

    if (applicationCount >= MAX_APPLICATIONS) {
        printf("\nApplication limit reached.\n");
        return;
    }

    showJobs();

    printf("\nEnter the Job ID you want to apply for: ");
    scanf("%d", &jobId);
    getchar();

    for (int i = 0; i < jobCount; i++) {
        if (jobs[i].id == jobId) {
            found = 1;

            printf("Enter your full name: ");
            fgets(applications[applicationCount].studentName,
                  sizeof(applications[applicationCount].studentName),
                  stdin);

            applications[applicationCount].studentName[
                strcspn(applications[applicationCount].studentName, "\n")
            ] = '\0';

            printf("Enter your email: ");
            fgets(applications[applicationCount].email,
                  sizeof(applications[applicationCount].email),
                  stdin);

            applications[applicationCount].email[
                strcspn(applications[applicationCount].email, "\n")
            ] = '\0';

            applications[applicationCount].jobId = jobId;
            applicationCount++;

            printf("\nApplication submitted successfully!\n");
            printf("Company: %s\n", jobs[i].company);
            printf("Role: %s\n", jobs[i].role);
            break;
        }
    }

    if (!found) {
        printf("\nInvalid Job ID.\n");
    }
}

void showApplications() {
    if (applicationCount == 0) {
        printf("\nNo applications submitted yet.\n");
        return;
    }

    printf("\n========== Submitted Applications ==========\n");

    for (int i = 0; i < applicationCount; i++) {
        int jobIndex = -1;

        for (int j = 0; j < jobCount; j++) {
            if (jobs[j].id == applications[i].jobId) {
                jobIndex = j;
                break;
            }
        }

        printf("\nApplication %d\n", i + 1);
        printf("Student: %s\n", applications[i].studentName);
        printf("Email  : %s\n", applications[i].email);

        if (jobIndex != -1) {
            printf("Company: %s\n", jobs[jobIndex].company);
            printf("Role   : %s\n", jobs[jobIndex].role);
        }
    }

    printf("\n=============================================\n");
}

int main() {
    int choice;

    printf("=============================================\n");
    printf("       ECE Career Connect Platform\n");
    printf("=============================================\n");

    do {
        printf("\n1. View jobs and internships\n");
        printf("2. Apply for a job\n");
        printf("3. View submitted applications\n");
        printf("4. Exit\n");
        printf("Choose an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                showJobs();
                break;

            case 2:
                applyForJob();
                break;

            case 3:
                showApplications();
                break;

            case 4:
                printf("\nThank you for using ECE Career Connect.\n");
                break;

            default:
                printf("\nInvalid option. Please try again.\n");
        }

    } while (choice != 4);

    return 0;
}
