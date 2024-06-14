# Deploy Blazor App to Amazon ECR in Docker Image
1. Start Windows Docker Service.. on Windows.
From project directory in Terminal, run:
1. aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 858915108625.dkr.ecr.eu-central-1.amazonaws.com
2. docker build -t 858915108625.dkr.ecr.eu-central-1.amazonaws.com/blazorapp1:latest -f Dockerfile .
3. docker push 858915108625.dkr.ecr.eu-central-1.amazonaws.com/blazorapp1:latest
Now the docker image is deployed in ECR..

4. ??? 4. aws cloudformation create-stack --stack-name blazorapp1-networking-infra --capabilities=CAPABILITY_NAMED_IAM --template-body file://ecs-infra.yaml --parameters file://ecs-infra.json
Result:
{                                                                                                                                                                                                                                                                                                       
"StackId": "arn:aws:cloudformation:eu-central-1:858915108625:stack/blazorapp1-networking-infra/7ad87d30-22c3-11ef-948e-0693a1e13715"
}


# Deploy Blazor App via .Zip
https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-publish
From project directory in Terminal, run:
1. dotnet publish -c Release 
2. Compress-Archive .\bin\Release\net8.0\publish\* .\Distributions\BlazorApp1.zip
or
3. Compress-Archive .\bin\Release\net8.0\publish\wwwroot\* .\Distributions\BlazorApp1.zip

.zip file is created in Distributions directory


#Amplify Delete App
Note, App Name is not AppId

In AWS, go to app, General Settings -> App Settings -> Delete App
Or
aws amplify list-apps
aws amplify delete-app --app-id d7tihh5p8d52h 



aws amplify create-app --name BlazorWasmApp2 --platform WEB --repository https://github.com/carlberg74/blank.git --access-token "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"  --custom-rules source="/<*>",target="/index.html",status="404-200" --build-spec "version: 1\nfrontend:\n  phases:\n    preBuild:\n      commands:\n        - curl -sSL https://dot.net/v1/dotnet-install.sh > dotnet-install.sh\n        - chmod +x *.sh\n        - ./dotnet-install.sh -c 8.0 -InstallDir ./dotnet8\n        - ./dotnet8/dotnet --version\n    build:\n      commands:\n        - ./dotnet8/dotnet publish -c Release -o release\n  artifacts:\n    baseDirectory: /release/wwwroot\n    files:\n      - '**/*'\n  cache:\n    paths: []"

Amplify kräver repository för att kunna sätta buildsettings, vilket krävs när man kör dotnet.
Så deploya en dotnet app (blazor) via .zip eller från S3 går inte.

Så sätt igång devops!




