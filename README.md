- 👋 Hi, I’m @johanegash
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
johanegash/johanegash is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->



//Yohannes Negash

I tried to worked in such like this

Kindly
It was difficult for me to open it the target file, so I tried to look at different websites
And I tried to do it according to what I understood
--->


#include <pcap.h>
#include <stdio.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>
#include <json.h>

#define SMB_HEADER_SIZE 32

typedef struct {
    char *source_ip;
    char *source_port;
    char *destination_ip;
    char *destination_port;
    char *filename;
    size_t file_size;
} smb_metadata_t;

int is_smbv2_packet(const u_char *packet) {
    // Check SMB2 signature (SMB Header, Network Byte Order)
    return (packet[0] == 0xFE) && (packet[1] == 0xFF);
}
int parse_smb_request(const u_char *packet, smb_metadata_t *metadata) {
    // Extract relevant information from SMB2 Write/Read Request header
    metadata->source_ip = "..."; // Implement source IP extraction
    metadata->source_port = "..."; // Implement source port extraction
    metadata->destination_ip = "..."; // Implement destination IP extraction
    metadata->destination_port = "..."; // Implement destination port extraction

    // SMB2 Command Code (offset 12)
    if (packet[12] == 0x08) { // SMB2_WRITE
        // ... (handle extraction, filename extraction, file size extraction)
    } else if (packet[12] == 0x0A) { // SMB2_READ
        // ... (handle extraction, filename extraction)
    } else {
        return -1; // Not a relevant SMB2 request
    }

    return 0; // Successful parsing
}
int parse_smb_response(const u_char *packet) {
    // Check for successful SMB2 response based on Status Code (offset 20)
    return packet[20] == 0x00; // Status Code = 0 indicates success
}
void extract_attachment(const u_char *packet, size_t offset, const char *filename) {
    // Extract attachment data from packet based on offset and write to file
    FILE *fp = fopen(filename, "wb");
    if (fp == NULL) {
        perror("fopen");
        return;
    }
    fwrite(packet + offset, 1, packet[1] - SMB_HEADER_SIZE, fp); // Adjust byte count based on packet length
    fclose(fp);
}

void create_metadata_json(smb_metadata_t *metadata, const char *filename) {
    // Create JSON object and write to file
    json_object *jobj = json_object_new_object();
    json_object_object_add(jobj, "source_ip", json_object_new_string(metadata->source_ip));
    json_object_object_add(jobj, "source_port", json_object_new_string(metadata->source_port));
    json_object_object_add(jobj, "destination_ip", json_object_new_string(metadata->destination_ip));
    json_object_object_add(jobj, "destination_port", json_object_new_string(metadata->destination_port));
    json_object_object_add(jobj, "filename", json_object_new_string(filename));
    json_object_object_add(jobj, "file_size", json_object_new_int(metadata->file_size));

    char json_filename[1024];
    snprintf(json_filename, sizeof(json_filename), "%s.json", filename);
    FILE *fp = fopen(json_filename, "w");
    if (fp == NULL) {
        perror("fopen");
        return;
    }
    fprintf(fp, "%s\n", json_object_to_json_string(jobj));
    fclose(fp);
    json_object_put(jobj);
}
int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <pcap_file>\n", argv[0]);
        return 1;
    }
[Yohannes Negash.txt](https://github.com/user-attachments/files/16088714/Yohannes.Negash.txt)
