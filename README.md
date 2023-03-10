# Distributed-Systems-Computations
Distributed MPI program written in C++ that is meant to make computations in a 4-cluster topology. Each cluster has an arbitrary number of worker processes.

- Coordinators read data about the cluster they are a part of
- Data is stored as a string, for simplicity

! Process 0 and 1 do not communicate. (communication channel error)

- Processes share the topology as a string as it follows:
  - 0->3->2->1
  - process 1 builds the full topology
  - starting with 1, every coordinator then sends the topology to all its
  workers and back to their coordinator neighbor until reaching 0 - 1->2->3->0

- For the computation of the array, process 0 builds the initial array.
- Processes share the array as it follows:
  - 0->3->2->1
  - at the same time, every coordinator that recieved the initial array,
  splits it in chunks and sends them to all of their workers
  - coordinators receive the results from workers at the same indexes they
  send them from
  - when a coordinator received all data from their workers, they wait for
  the next coordinator to send them the processed data and replace it in the
  array, then forwards it -> in the way of process 0 (MASTER): 1->2->3->0

