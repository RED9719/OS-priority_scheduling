# OS-priority_scheduling

import pandas as pd

class Process:
    def __init__(self, pid, arrival_time, burst_time, priority):
        self.pid = pid
        self.arrival_time = arrival_time
        self.burst_time = burst_time
        self.priority = priority
        self.start_time = 0
        self.completion_time = 0
        self.turnaround_time = 0
        self.waiting_time = 0

def priority_scheduling(processes):
    processes.sort(key=lambda x: (x.arrival_time, x.priority))

    current_time = 0
    completed = 0
    n = len(processes)
    while completed != n:
        ready_queue = [p for p in processes if p.arrival_time <= current_time and p.completion_time == 0]
        if ready_queue:
            ready_queue.sort(key=lambda x: x.priority)
            process = ready_queue[0]
            process.start_time = current_time
            process.completion_time = process.start_time + process.burst_time
            process.turnaround_time = process.completion_time - process.arrival_time
            process.waiting_time = process.turnaround_time - process.burst_time
            current_time = process.completion_time
            completed += 1
        else:
            current_time += 1

def print_process_info(processes):
    process_info = []
    for process in processes:
        process_info.append([process.pid, process.arrival_time, process.burst_time,
                             process.priority, process.start_time, process.completion_time,
                             process.turnaround_time, process.waiting_time])
    
    df = pd.DataFrame(process_info, columns=["Process ID", "Arrival Time", "Burst Time",
                                             "Priority", "Start Time", "Completion Time",
                                             "Turnaround Time", "Waiting Time"])
    print(df)

def main():
    processes = []
    num_processes = int(input("Enter the number of processes: "))

    for i in range(num_processes):
        pid = int(input(f"\nEnter Process ID for process {i + 1}: "))
        arrival_time = int(input(f"Enter Arrival Time for process {pid}: "))
        burst_time = int(input(f"Enter Burst Time for process {pid}: "))
        priority = int(input(f"Enter Priority for process {pid} (lower number indicates higher priority): "))
        processes.append(Process(pid, arrival_time, burst_time, priority))

    priority_scheduling(processes)
    print("\nProcess Information after Priority Scheduling:")
    print_process_info(processes)

if __name__ == "__main__":
    main()
