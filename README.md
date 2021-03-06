TNKernel-PIC32
==============

A port of [TNKernel](http://www.tnkernel.com/ "TNKernel") for PIC32

---
TNKernel-PIC32 is a new, independent port of TNKernel 2.6, based on the Cortex-M3 version.

##Context switch
The context switch is implemented using the core software 0 interrupt. It should be configured to use the lowest priority in the system:

    // set up the software interrupt 0 with a priority of 1, subpriority 0
    INTSetVectorPriority(INT_CORE_SOFTWARE_0_VECTOR, INT_PRIORITY_LEVEL_1);
    INTSetVectorSubPriority(INT_CORE_SOFTWARE_0_VECTOR, INT_SUB_PRIORITY_LEVEL_0);
    INTEnable(INT_CS0, INT_ENABLED);

The interrupt priority level used by the context switch interrupt should not be configured to use shadow register sets.

##Interrupts
TNKernel-PIC32 supports nested interrupts. The kernel provides assembly-language macros for calling C-language interrupt service routines. Both software and shadow register interrupt context saving is supported:

    #include "tn_port_asm.h"
    
    #define _CORE_TIMER_VECTOR 0
    #define _INT_UART_1_VECTOR 24
    
    # Core timer interrupt handler using software interrupt context saving
    tn_soft_isr CoreTimerHandler _CORE_TIMER_VECTOR
    
    # High-priority UART interrupt handler using shadow register set
    tn_srs_isr UartHandler _INT_UART_1_VECTOR

##Interrupt stack
TNKernel-PIC32 supports using a separate stack for interrupt handlers. The feature is enabled by adding `TN_INT_STACK` to the preprocessor definitions in your project (for both C and assembly language files). Switching stack pointers is done automatically in the ISR handler wrapper macros. The size of the interrupt stack in words is configured in `tn_port.h`:

    #define  TN_INT_STACK_SIZE         256
 
---

The kernel is released under the BSD license as follows:

    TNKernel real-time kernel

    Copyright © 2004, 2010 Yuri Tiomkin
    PIC32 version modifications copyright © 2013 Anders Montonen
    All rights reserved.

    Permission to use, copy, modify, and distribute this software in source
    and binary forms and its documentation for any purpose and without fee
    is hereby granted, provided that the above copyright notice appear
    in all copies and that both that copyright notice and this permission
    notice appear in supporting documentation.

    THIS SOFTWARE IS PROVIDED BY THE YURI TIOMKIN AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
    IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
    ARE DISCLAIMED. IN NO EVENT SHALL YURI TIOMKIN OR CONTRIBUTORS BE LIABLE
    FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
    DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
    OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
    HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
    LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
    OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
    SUCH DAMAGE.
