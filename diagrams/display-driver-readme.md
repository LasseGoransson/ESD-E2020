# Display Driver Task README

The `DisplayDriverTask` class encompasses the thread necessary to concurrently interact with the Display hardware. It inherits from the `Task` class and uses the `DisplayDriver` class to communicate with the hardware. It provides data structures and high level methods to easily send data to the display.

```cpp
#include "DisplayDriverTask.h"

// instantiate the display
DisplayDriverTask disp();

// try starting the thread
if (not disp.start())
{
    std::cerr << "Could not start DisplayDriverTask." << std::endl;
    exit(-1);
}

// use high-level API to print data
DisplayLines lines = { "Hello", "world!" };
disp::print(lines);

// or lazy dog style
disp::print({ "Hello", "world!" });

// use mqueue to print data
DisplayDriverTask::Msg msg = { DisplayDriverTask::Command::PRINT, &lines };

auto qd = mq_open(DisplayDriverTask::QUEUE_NAME, O_WRONLY))
mq_send(qd, (const char*) &msg, sizeof(msg), 0);
```

