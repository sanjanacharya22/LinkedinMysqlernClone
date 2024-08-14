#!/bin/bash

# Define variables
USERNAME="sanjanacharya22"
TOKEN=""
REPO_URL="https://$USERNAME:$TOKEN@github.com/sanjanacharya22/LinkedinMysqlernClone.git"
PROJECT_DIR="/www/wwwroot/lnkin"
BRANCH="main"  # or master
FRONTEND_DIR="$PROJECT_DIR/frontend"
BACKEND_DIR="$PROJECT_DIR/backend"

# Export GIT_ASKPASS to avoid manual credential entry
export GIT_ASKPASS=$(mktemp)
echo "echo $TOKEN" > "$GIT_ASKPASS"
chmod +x "$GIT_ASKPASS"

# Clone the repository if it doesn't exist
if [ ! -d "$PROJECT_DIR/.git" ]; then
  echo "Cloning repository from $REPO_URL..."
  git clone $REPO_URL $PROJECT_DIR
  cd $PROJECT_DIR
  git checkout $BRANCH
else
  echo "Repository already exists. Skipping clone."
fi

# Pull latest code from Git
echo "Pulling latest code from Git..."
cd $PROJECT_DIR
git fetch --all
git reset --hard origin/$BRANCH
git pull origin $BRANCH

# Install dependencies for the backend (Express.js)
echo "Installing backend dependencies..."
cd $BACKEND_DIR
npm install

# Build and Install dependencies for the frontend (React.js)
echo "Installing frontend dependencies and building the project..."
cd $FRONTEND_DIR
npm install
npm run build

# Move the built React app to the server's public directory (if applicable)
# This step depends on how you serve your React app
#echo "Moving built frontend files to public directory..."
#cp -R $FRONTEND_DIR/build/* /var/www/html/yourproject

# Restart the backend server using PM2
echo "Restarting backend server..."
cd $BACKEND_DIR
pm2 restart server.js --name "yourproject-backend"

echo "Deployment complete!"
