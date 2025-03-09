# SPMC PubSubQueue (Variable Type)

+ For an SPMC (Single Producer Multiple Consumers) PubSubQueue, the core design idea is that each consumer thread stores its own read index. Additionally, the producer does not check whether the queue is full, ensuring that consumers do not affect the producer.

+ If we want to store messages of arbitrary length in the queue, we can divide the queue into units of MsgHeader. When publishing a message, first fill in the message header, specify the length in the header, and then store the message body after the header, aligned to the MsgHeader. When reading, simply read the header first to get the message length, and then read the message body according to the length.

+ After a consumer reads the message header, it must perform a series of checks to ensure that the header is not corrupted.