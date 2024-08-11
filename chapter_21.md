# chapter 21 (transactions)

## single-user vs multi-user

- single-user: at most only one user uses system at time
- multi-user: multiple users can use system at the same time, all access database at the same time- (concurrently)

## how concept of multi-user works

- it is managed using single processor or multiple processors
- using single processor:
    - it uses the concept of multiprogramming which allow same processor to switch between different tasks
- using multiple processors:
    - it uses the concept of parallel processing
    - every processor works in different task in parallel

## data item

- anything i read from database
- data item may be record, disk block or an attribute

## read_item(x)

- read item from database, and store it in program variable called x
- steps for read:
    1. find address of block that contains variable x, index tells me what it the address of the block
    2. copy the whole block that contains item x into the buffer in main memory
       (if that disk block is not already in main memory)
        - i transfer whole block to buffer because accessing disk takes a lot of time
          an i may need other items in that block, in other operations
    3. copy value of item x into program variable x

## write_item(x)

- store value of program variable x in database
- steps for write:
    1. find address of block that contains item x, index tells me what it the address of the block
    2. copy the whole block that contains item x into the buffer in main memory
       (if that disk block is not already in main memory)
    3. copy item x from program variable into correct place in the buffer
    4. store updated block from the buffer to the disk

## why concurrency control is needed?
- to control access to database
- many operations may need to access database concurrently
- concurrency control help us to make sure that all operations are applied correctly
### problems that may arise if concurrency control is not used
1. lost update
   - in some case there may be two transaction update one item at the same time
   - every one will update its value
   - the last one will overwrite all other transactions and not sense their updates
   - concurrency control solve that problem by making sure that only one transaction update item at time
2. temporary update
   - may be one transaction work in item and update its value
   - another transaction reads that updated value after it is updated
   - the first transaction fails, and the old value returned back
   - that second transaction will must fail too, it now works on wrong value
3. incorrect summary problem 
   - if one transaction calculating one of the aggregate functions while the other is updating the item
   - may be value is calculated then the item updated
   - so it will give incorrect value
   - so concurrency control must solve that by making sure that no item updated while aggregate is calculated
4. unrepeatable read problem
   - there is a transaction that reads item x twice
   - there is another transaction that updates item x while the interval between two reads
   - the first transaction will fail
## Recovery
- when interrupt occurs database will return back to its old state before interrupt
### types of failure
1. computer failure (system crash)
   - hardware
   - software
   - network
2. transaction or system error
   - overflow
   - division by zero
3. local errors or exception conditions detected by transaction
   - insufficient balance
4. concurrency control enforcement
5. Disk failure
   - maybe we lose data while reading or write from disk
6. physical problems or catastrophes

## transaction states
- active => between begin and end
- partially committed => at the end of the transaction, before say that transaction is committed and apply its changes to database
- committed => at the end of the transaction, after partially committed, when that transaction changes are applied to database
- failed => at the end of the transaction, after partially committed, when i find error in transaction and decide to cancel its changes
- terminated => end