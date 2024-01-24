# **Encryption and Decryption using Ceasar Cipher**  
An effective system is made to encrypt and then decrypt a plain text message effectively.  
We used the Irvine32 Library given by Kipp Irvine for developing in assembly language.  
This program effectively converts the plain text message given by the user to encrypted text using the Ceasar Shift Algorithm. The value of key can be changed before running the code. As the text is encrypted then the key is required to effectively decrypt the cipher text and to also change the plain text.  
The Code is given below in the Code heading for reference.

## **Challenges Faced**  
As it was developed in assembly language with my other teammates had to work very carefully as we were directly working with regiser to handle data, a slight change caused the code to collapse and errors to occur.  

## **Code**  

INCLUDE Irvine32.inc  

.data  
plaintext   BYTE 100 DUP(?) ;To store the plain text  
ciphertext  BYTE SIZEOF plaintext DUP(0) ;To Store the Cipher Text  
  
key         DWORD 3 ;The predefined key  
key1        DWORD ?   
checkv      DWORD ?  

promptplain BYTE "Enter the plain text: ",0  
keyPrompt   BYTE "Enter the key: ", 0  
invalidKey  BYTE "Invalid key! Please enter key again : ", 0  
checktext   BYTE "Enter 1 to show decrypted message ",0  
checktext2  BYTE "Enter 2 to show encrypted message ",0  
checktext3  BYTE "Enter 3 to change plain text ",0  
checktext1  BYTE "Enter 4 to exit : ",0  
encryptedMsg BYTE "Encrypted Text: ", 0  
encryptsuccess BYTE "Encryption Successful ", 0  
decryptedMsg BYTE "Decrypted Text: ", 0  
decryptsuccess BYTE "Decryption Successful ", 0  
  
.code  
main PROC  
      
    ent_plain_again:
         ; Taking plain text from user
         call Clrf
         mov edx, OFFSET promptplain
         call WriteString
         mov edx, OFFSET plaintext
         mov ecx, SIZEOF plaintext
         call ReadString

         ; Encrypting the plain text with the predefined key
         mov  esi, OFFSET plaintext
         mov  edi, OFFSET ciphertext
         mov  ecx, SIZEOF plaintext
         movzx ebx, byte ptr key
         call CaesarEncrypt

         ; Display the encryption success message
         mov edx, OFFSET encryptsuccess
         call WriteString
         call Crlf
         call Crlf

         ; Taking the key from the user to validate it
         input_key:
             mov edx, OFFSET keyPrompt
             call WriteString
             call ReadInt
             mov key1, eax

             ; Validating the key
             mov eax, key
             mov ebx, key1
             cmp eax, ebx
             jne invalid_key

            dec_enc_:
                 ; Asking User if to display the Decrypted or Encrypted text
                 call Crlf
                 mov edx, OFFSET checktext
                 call WriteString
                 call Crlf
                 mov edx, OFFSET checktext2
                 call WriteString
                 call Crlf
                 mov edx, OFFSET checktext3
                 call WriteString
                 call Crlf
                 mov edx, OFFSET checktext1
                 call WriteString
                 call ReadInt
                 mov checkv, eax

                 cmp checkv, 2
                 je display_cipher_text
                 cmp checkv, 1
                 je decryptt
                 cmp checkv, 3
                 je ent_plain_again
                 jg end_program

         invalid_key:
             ; Display an error message and prompt to enter key again in case of invalid key
             mov edx, OFFSET invalidKey
             call WriteString
             jmp input_key

         decryptt:
             mov  esi, OFFSET ciphertext
             mov  edi, OFFSET plaintext
             mov  ecx, SIZEOF ciphertext
             movzx ebx, byte ptr key
             call CaesarDecrypt
             call Crlf
             mov edx, OFFSET decryptedMsg
             call WriteString
             mov edx, OFFSET plaintext
             call WriteString
             call Crlf
             jmp dec_enc_

         display_cipher_text:
             call Crlf
             mov edx, OFFSET encryptedMsg
             call WriteString
             mov edx, OFFSET ciphertext
             call WriteString
             call Crlf
             jmp dec_enc_

         end_program:
             call Crlf
             call WaitMsg
             call ExitProcess
main ENDP  
  
; Function to encrypt the text using Caesar cipher  
CaesarEncrypt PROC  
    mov  al, 0  
    movzx ebx, byte ptr key  
  
encrypt_loop:  
    cmp  BYTE PTR [esi], 0  ; Check for the null terminator  
    je   encrypt_done  
  
    mov  al, [esi]
    add  al, bl
    mov  [edi], al

    inc  esi
    inc  edi
    loop encrypt_loop

encrypt_done:  
    ret  
CaesarEncrypt ENDP  
  
CaesarDecrypt PROC  
    mov  eax, 0  
    movzx ebx, byte ptr key  
  
decrypt_loop:  
    cmp  BYTE PTR [esi], 0  ; Check for the null terminator  
    je   decrypt_done  
  
    mov  al, [esi]
    sub  al, bl
    mov  [edi], al

    inc  esi
    inc  edi
    loop decrypt_loop

decrypt_done:  
    ret  
CaesarDecrypt ENDP  
  
END main  
