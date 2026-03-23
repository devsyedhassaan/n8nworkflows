# n8nworkflows
n8n Automation Workflows for AI Chatbots
[Chatbot appointment scheduler with Google Calendar for dental assistant.json](https://github.com/user-attachments/files/26175183/Chatbot.appointment.scheduler.with.Google.Calendar.for.dental.assistant.json)
{
  "name": "Chatbot appointment scheduler with Google Calendar for dental assistant",
  "nodes": [
    {
      "parameters": {
        "public": true,
        "initialMessages": "Hi there! 👋\nMy name is SMH Bot. How can I assist you today?",
        "options": {}
      },
      "id": "32fe7a8b-aa1a-4517-a167-41972f77d69b",
      "name": "When chat message received",
      "type": "@n8n/n8n-nodes-langchain.chatTrigger",
      "position": [
        432,
        224
      ],
      "webhookId": "8f427031-1110-4ea3-aef7-5d06ba7d5bce",
      "typeVersion": 1.1
    },
    {
      "parameters": {
        "options": {
          "systemMessage": "=🎯 Role of the Assistant\nYou are a virtual assistant specializing in appointment management for Dr. Trenton. Your goal is to schedule consultations accurately, ensuring real availability while providing a smooth experience for patients.\n\n🕒 Office Hours\nMonday - Friday: 8:00 AM - 6:00 PM\nSaturday: 8:00 AM - 1:00 PM\nSunday: ❌ Closed\nConsultation Duration: 1 hour\nBreak Between Patients: 15 minutes\n\n\nWhenever Anyone start chat, reply with this:\n\n\"Hello! How can I assist you today? If you're looking to schedule an appointment with Dr. Trenton, please provide your full name, phone number, and the desired date and time for your consultation.\"\n\nAbout Dr. Trenton:\nDr. Trenton is the Medical Director of the Heart Transplant Service at Weill Cornell Medicine. He specializes in the care of patients with heart failure, patients requiring or who have a heart transplant or ventricular assist device (LVAD), and patients with pulmonary hypertension. \n\n\n📅 Booking Process\n\n1️⃣ Request Patient Information (Mandatory):\nFull Name\nPhone Number\nDesired Date and Time\n2️⃣ Availability Check:\nIf the requested time is outside office hours → offer only available slots.\nIf the requested time is available, ask for confirmation and book it.\nIf the requested time is unavailable, apologize and suggest the actual available slots on the requested day (between 8:00 AM and 6:00 PM, respecting breaks).\n\n##Example:\nIf a patient requests an appointment at 10:00 AM, check Google Calendar to confirm availability between 8:00 AM and 6:00 PM, considering the consultation duration (1 hour) and the 15-minute breaks.\n\n🚨 Do not confirm the appointment immediately—you must receive the patient's confirmation first.\n\n3️⃣ Confirmation & Updates:\nConfirm availability with the patient before finalizing.\nUpdate Google Calendar & Google Sheets after every booking.\nGoogle Calendar Event Title: \"Patient Name - Phone Number\".\nFor modifications or cancellations, free the slot and update the schedule.\n\n##Tools:\nUse \"Cheek Avilability\" to check available slots.\nUse \"Creat event\" to book the appointment.\nUse \"Add Data\" to record patient information.\n\n💬 Communication\n✅ Respond clearly, professionally, and in a friendly manner.\n✅ Always confirm the final date and time with the patient.\n✅ Ensure Google Calendar and Google Sheets are updated after every booking.\n\n📅 Today's date: {{ $now }}."
        }
      },
      "id": "3510bb5a-3c8b-4978-a6c5-5c077be74f3f",
      "name": "AI Agent",
      "type": "@n8n/n8n-nodes-langchain.agent",
      "position": [
        720,
        224
      ],
      "typeVersion": 1.7
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $('When chat message received').item.json.sessionId }}"
      },
      "id": "05bfbeb4-d2a4-4372-b763-6da636ed4393",
      "name": "Window Buffer Memory",
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "position": [
        768,
        448
      ],
      "typeVersion": 1.3
    },
    {
      "parameters": {
        "resource": "calendar",
        "calendar": {
          "__rl": true,
          "value": "applicationscorner21@gmail.com",
          "mode": "list",
          "cachedResultName": "applicationscorner21@gmail.com"
        },
        "timeMin": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Start_Time', ``, 'string') }}",
        "timeMax": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('End_Time', ``, 'string') }}",
        "options": {
          "timezone": {
            "__rl": true,
            "value": "Asia/Karachi",
            "mode": "id"
          }
        }
      },
      "id": "398fdf7a-508d-4a0a-8c2c-1f0075b6ad56",
      "name": "Check Availability",
      "type": "n8n-nodes-base.googleCalendarTool",
      "position": [
        944,
        448
      ],
      "typeVersion": 1.3,
      "credentials": {
        "googleCalendarOAuth2Api": {
          "id": "d0qc94Bq0bk3oo5h",
          "name": "Google Calendar OAuth2 API"
        }
      }
    },
    {
      "parameters": {
        "calendar": {
          "__rl": true,
          "value": "applicationscorner21@gmail.com",
          "mode": "list",
          "cachedResultName": "applicationscorner21@gmail.com"
        },
        "start": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Start', ``, 'string') }}",
        "end": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('End', ``, 'string') }}",
        "additionalFields": {}
      },
      "id": "43673597-ffb6-4d38-8fb0-975eb47976f6",
      "name": "Creat event",
      "type": "n8n-nodes-base.googleCalendarTool",
      "position": [
        1088,
        448
      ],
      "typeVersion": 1.3,
      "credentials": {
        "googleCalendarOAuth2Api": {
          "id": "d0qc94Bq0bk3oo5h",
          "name": "Google Calendar OAuth2 API"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1ygaNwxhp4Sfe2KP7LkJv-8Pa6koCtHWN_t9LEj_gIXI",
          "mode": "list",
          "cachedResultName": "Email Scraped",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1ygaNwxhp4Sfe2KP7LkJv-8Pa6koCtHWN_t9LEj_gIXI/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1390494175,
          "mode": "list",
          "cachedResultName": "Appoitnment with Dr. SM",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1ygaNwxhp4Sfe2KP7LkJv-8Pa6koCtHWN_t9LEj_gIXI/edit#gid=1390494175"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Phone Number": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Phone_Number', ``, 'string') }}",
            "Appointment Time": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Appointment_Time', ``, 'string') }}",
            "Patient Name": "={{ /*n8n-auto-generated-fromAI-override*/ $fromAI('Patient_Name', ``, 'string') }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "Patient Name",
              "displayName": "Patient Name",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Phone Number",
              "displayName": "Phone Number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "Appointment Time",
              "displayName": "Appointment Time",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "id": "650b5d36-7b52-4bb2-953a-d9ee278a35eb",
      "name": "Add data",
      "type": "n8n-nodes-base.googleSheetsTool",
      "position": [
        1232,
        448
      ],
      "typeVersion": 4.5,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "MOg7xVLyxZ4XKYSB",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "model": "openai/gpt-4o-mini",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenRouter",
      "typeVersion": 1,
      "position": [
        592,
        432
      ],
      "id": "e5a4fe74-7d36-4f0e-a177-0036c6a74539",
      "name": "OpenRouter Chat Model",
      "credentials": {
        "openRouterApi": {
          "id": "1uvkL4qVu7AAtCha",
          "name": "OpenRouter account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "AI Agent": {
      "main": [
        []
      ]
    },
    "Add data": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Creat event": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Check Availability": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    },
    "Window Buffer Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "When chat message received": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenRouter Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": false
  },
  "versionId": "419ddade-9aa3-4138-b9be-aff66ac1c0fb",
  "meta": {
    "templateId": "3131",
    "templateCredsSetupCompleted": true,
    "instanceId": "658099e00cbf74e2aa2cec7eef679163c9224071bc963ee1a8dbeea7df9bf641"
  },
  "id": "zpXZ9qGMLR6MMonr",
  "tags": []
}
