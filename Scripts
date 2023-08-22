#!/bin/bash

#To automate the process of removing or deleting Macs that haven't checked in for over XX days
#
#
#
#
#
#Tested and created by Mr. Jaykrishna Kumar

# Set the MDM API endpoint and authentication credentials
API_URL="https://your-mdm-api-endpoint"
API_KEY="your-api-key"

# Calculate the date XX days ago
NINETY_DAYS_AGO=$(date -v -XXd "+%Y-%m-%d")

# Get the list of devices from the MDM
DEVICE_LIST=$(curl -s -H "Authorization: Bearer $API_KEY" "$API_URL/devices")

# Loop through the devices and remove those not checked in for over XX days
for DEVICE in $(echo "$DEVICE_LIST" | jq -r '.[] | @base64'); do
    DEVICE_DATA=$(echo "$DEVICE" | base64 --decode | jq -r '.')
    LAST_CHECK_IN=$(echo "$DEVICE_DATA" | jq -r '.last_check_in')  # Adjust the key as needed

    if [[ "$LAST_CHECK_IN" < "$NINETY_DAYS_AGO" ]]; then
        DEVICE_ID=$(echo "$DEVICE_DATA" | jq -r '.id')  # Adjust the key as needed
        curl -X DELETE -H "Authorization: Bearer $API_KEY" "$API_URL/devices/$DEVICE_ID"
        echo "Removed device: $DEVICE_ID"
    fi
done
