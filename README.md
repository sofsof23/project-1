#!/bin/bash
# Run this script locally to push index.html to GitHub

TOKEN="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"
REPO="sofsof23/project-1"
FILE="index.html"
CONTENT=$(base64 -w 0 "$FILE")

SHA=$(curl -s -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/$FILE" \
  | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))" 2>/dev/null)

if [ -n "$SHA" ]; then
  BODY=$(python3 -c "import json; print(json.dumps({'message': 'Add Claude Chat vs Claude Code comparison website', 'content': '$CONTENT', 'sha': '$SHA'}))")
else
  BODY=$(python3 -c "import json; print(json.dumps({'message': 'Add Claude Chat vs Claude Code comparison website', 'content': '$CONTENT'}))")
fi

curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Content-Type: application/json" \
  -d "$BODY" \
  "https://api.github.com/repos/$REPO/contents/$FILE"

echo "Done!"
