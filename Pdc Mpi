#include <mpi.h>
#include <cstdio>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int myid, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &myid);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int range_start, range_end;
    if (myid == 0) {
        // Process 0 gets user input for the range
        printf("Enter the start of the range: ");
        std::scanf("%d", &range_start);
        printf("Enter the end of the range: ");
        std::scanf("%d", &range_end);
    }

    // Broadcast the range to all processes
    MPI_Bcast(&range_start, 1, MPI_INT, 0, MPI_COMM_WORLD);
    MPI_Bcast(&range_end, 1, MPI_INT, 0, MPI_COMM_WORLD);

    // Divide the range among processes
    int numbers_per_process = (range_end - range_start + 1) / size;
    int start = range_start + myid * numbers_per_process;
    int end = start + numbers_per_process - 1;
    if (myid == size - 1) end = range_end; // Last process handles the remaining numbers

    // Find the maximum value in this range
    int local_max = start;
    for (int num = start; num <= end; ++num) {
        if (num > local_max) local_max = num;
    }

    // Process 0 collects the maximum values and computes the global max
    int global_max;
    MPI_Reduce(&local_max, &global_max, 1, MPI_INT, MPI_MAX, 0, MPI_COMM_WORLD);

    if (myid == 0) {
        printf("The maximum value in the range is: %d\n", global_max);
    }

    MPI_Finalize();
    return 0;
}
