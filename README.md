The code is compiled from this source code in C# :
```cs
using System;
using System.Threading;
using System.Runtime.InteropServices;
using System.Windows.Forms;

namespace SimulationForensic
{
    class Program
    {
        static volatile string secretFlag = "MALWARE{y0u hAV€ b&en P&nwd}";

        [STAThread]
        static void Main(string[] args)
        {
            string memoryAnchor = secretFlag; 

            MessageBox.Show("You are under attack !!\n\n(Ceci est une simulation forensic)", 
                            "CRITICAL ALERT", 
                            MessageBoxButtons.OK, 
                            MessageBoxIcon.Warning);
        }
    }
}
```
