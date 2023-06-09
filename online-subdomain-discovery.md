# Find Subdomains Online


# Parse RapidDNS Data
```CSS
cat rapiddns_data.csv | grep -i "cname" | awk -F ',' '{print $2}' > cname_records.data
```
```CSS
cat rapiddns_data.csv | grep -i "cname" | awk -F ',' '{print $3}' | sed 's/\.$//' >> cname_records.data 
```
```CSS
cat rapiddns_data.csv | grep -i -v "cname" | awk -F ',' '{print $2}' >> a_records.data
```
```CSS
cat cname_records.data a_records.data | sort -u >> hosts.txt
```

Script:
```SHELL
#!/bin/bash

if [ $# -ne 1 ]; then
  echo "Usage: $0 <csv_file>"
  exit 1
fi

csv_file=$1
filename=$(basename "$csv_file" .csv)
cname_records_file="${filename}_cname_records.data"
a_records_file="${filename}_a_records.data"
hosts_file="${filename}_hosts.txt"

grep -i "cname" "$csv_file" | awk -F ',' '{print $2}' > "$cname_records_file"
grep -i "cname" "$csv_file" | awk -F ',' '{print $3}' | sed 's/\.$//' >> "$cname_records_file"
grep -i -v "cname" "$csv_file" | awk -F ',' '{print $2}' >> "$a_records_file"
cat "$cname_records_file" "$a_records_file" | sort -u >> "$hosts_file"

echo "Extraction Complete!"
```
Usage:
```SHELL
./extractor.sh rapiddns_data.csv
```
