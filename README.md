# cpuinfo
This program is used to obtain cpu id

用于识别cpu的id，点击右上角的code选择下载，并参照使用说明运行该程序。为了使大家放心使用，我们将程序代码公示如下：

```
#include <arpa/inet.h>
#include <fstream>
#include <iostream>
#include <string.h>
#include <unistd.h>

class HardwareInfo {
public:
  HardwareInfo();
  static std::string getCpuId() {
    std::string cpu_id;
    unsigned int s1 = 0;
    unsigned int s2 = 0;

    asm volatile("movl $0x01, %%eax; \n\t"
                 "xorl %%edx, %%edx; \n\t"
                 "cpuid; \n\t"
                 "movl %%edx, %0; \n\t"
                 "movl %%eax, %1; \n\t"
                 : "=m"(s1), "=m"(s2));

    if ((0 == s1) && (0 == s2)) {
      return std::string(cpu_id);
    }

    char cpu[128];
    memset(cpu, 0, sizeof(cpu));
    snprintf(cpu, sizeof(cpu), "%08X%08X", htonl(s2), htonl(s1));
    cpu_id.assign(cpu);

    return std::string(cpu_id);
  }
};

int main(int argc, char *argv[]) {
  auto cpuId = HardwareInfo::getCpuId();
  std::cout << "CPU Id: " << cpuId << std::endl;
  return 0;
}
```
