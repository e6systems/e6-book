# Caution

UEFI involves a lot of dereferencing and indirection, so when writing a UEFI application in assembly language (in this case NASM) the square brackets must be precisely and accurately placed. The PE header of the installer's code illustrates this in the DOS stub field:
```
.DOS_SIGNATURE:              db "MZ"                                                             ; DOS Signature. This is required
    ; A DOS stub should normally be here but uefi doesn't need it, so this will be filled
    ; with something a bit more useful.
    .pre_start: 
        push rbx
        lea rbx, [DATA_DIRECTORIES.EFI_IMAGE_HANDLE]        ; We're going to store the efi image handle
        mov [rbx], rcx                                      ; Store the efi image handle
        add rbx, 8                                          ; Point to the memory address to store the system table
        mov [rbx], rdx                                      ; Store the pointer to the system table

        ; Point to the simple text output protocol
        add rdx, EFI_SIMPLE_TEXT_OUTPUT_PROTOCOL 
        mov rdx, [rdx]
        mov rcx, rdx                                        ; Killing 2 birds with 1 stone. This happens to be the first parameter for Output String
        add rdx, EFI_SIMPLE_TEXT_OUTPUT_PROTOCOL_OutputString
        mov rbx, [rdx]
        lea rdx, [OPTIONAL_HEADER_START.BOOT_MESSAGE]
        sub rsp, 32
        call rbx
        add rsp, 32
        jmp $

        ; Print the ins
        pop rbx
        ret

        times 60 - ($ - STANDARD_HEADER) db 0    
```
