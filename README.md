# ASP.NET Core with JetBrains Rider/Visual Studio & Sql Server on Mac Intel/M1

## Video Demo
[SQL Server + Jetbrains Rider for Mac (M1/Intel) - Updated for 2022 - Detailed Demo](https://youtu.be/CWBNCWxD_0c)

[SQL Server + Visual Studio for Mac (M1/Intel) - Updated for 2022 - Detailed Demo](https://www.youtube.com/watch?v=bbng41TLl8M)

## Disclaimer
This is purely for sharing purpose. If you encounter any issues, please kindly do your due diligence by Googling them first. I'm new to this and still learning too.

## ⓵ Get Docker on Mac with an SqlServer Docker image

Based on a video tutorial by [Valuetech Academy](https://www.youtube.com/channel/UCwN4XNYmbPL8IRPe-E0BnYQ): https://www.youtube.com/watch?v=o5r3kFnsL-Q
	
1. Download and install Docker on Mac (Intel/Apple chip version based on your Mac's architecture)

2. Open Docker app, enter your Mac password to install helper, log in to your account

3. Open Docker Preferences, raise the system RAM resource to 4GB (advised by the video tutorial) -> Click OK then wait for Docker to restart

4. Open Terminal

	a. If M1 Mac, type and enter `docker pull mcr.microsoft.com/azure-sql-edge:latest`
	
	b. If Intel Mac, type and enter `docker pull microsoft/mssql-server-linux`
	
5. Choose any password that **suits SqlServer's requirements** (at least 8 chars with lowercase, uppercase, numbers, special symbols), e.g. Docker@123. Then still in Terminal

	a. If M1 Mac,
		type and enter `docker run -d --name ms-sql-server -e "ACCEPT_EULA=Y" -e 'SA_PASSWORD=Docker@123' -p 1433:1433 mcr.microsoft.com/azure-sql-edge:latest`
		
	b. If Intel Mac,
		type and enter `docker run -d --name ms-sql-server 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Docker@123' -p 1433:1433 microsoft/mssql-server-linux`
	
	*Note: If you chose a different password, update the SA_PASSWORD part accordingly*
		
-> If Docker runs it successfully, you'll see a long hash number displayed without any WARNING. Then to double check, head over to Docker > Containers / Apps, you'll see a Docker image with name `mssql` running with a green status.

Otherwise, please refer to the comment section of the video tutorial to find out more workarounds for M1 Mac environment.

## ⓶ Configure ASP.NET Core project to work with the database

*Note: This should work similarly with an alternative IDE. Here I'm using JetBrains Rider, but conceptually you can work out the logic with Visual Studio for Mac too. Otherwise please watch the [demo for Visual Studio](https://www.youtube.com/watch?v=bbng41TLl8M) I listed above*

1. Update the target framework of your ASP.NET Core project to the latest available SDK (e.g. .NET 6.0)

2. Update your currently installed NuGet packages to the corresponding target SDK (e.g. 6.0.x)

3. *👉 This step 3, and step 4 below is **IMPORTANT**! It's the very part where you __*actually*__ configure your current ASP.NET Core project to work with your Docker mssql server. Now, you would edit your `appsettings.json` database connection string.*
	
	***For example: say your app's database context is named `ShopContext.cs`. Then in `appsettings.json`, you would edit the connection string as follows:***
	```json
	"ConnectionStrings": {
		"ShopContext": "Server=(localdb)\\mssqllocaldb;Database=GuitarShop;Trusted_Connection=True;MultipleActiveResultSets=true"
    	/* Back up the above string somewhere, then update it as below with corresponding Database/Password info ... */
		"ShopContext": "Server=localhost;Database=GuitarShop;User=sa;Password=Docker@123;"
  	}
	```
	
	*⚠️⚠️⚠️ Please note to **NOT** commit this `appsettings.json` edit to your group's repo, as it might break your group members' since you're using a Mac and they're on Windows. This workaround should be done locally on your macOS environment only.*
	
	*Also, if you chose a different password, change the Password part accordingly*

4. Open Terminal, make sure you installed the EF CLI tools already. Otherwise google `install dotnet ef tools`. Then in Terminal directed to the project solution directory

	a. If the sample code comes with migrations already, just perform an update on your current database. To do that, type and enter `dotnet ef database update` -> Wait till the console says `Done.`
	
	b. If the sample code doesn't come equipped with migrations, you could try adding them manually. To do that, type and enter `dotnet ef migrations add <your_migration_name>`. Then go back to step 4a above.
	
        
 ## ⓷ Run your ASP.NET Core App 🥳
 This is the final step (albeit not necessarily needed for this tutorial), build and run the app.
 That's the whole tutorial there. Good luck!
 
 ## P/S:
 Additionally, please also check out this article to [install the ODBC driver for SQL Server on macOS](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos?view=sql-server-ver15) if you encounter any driver-related issues.
 
