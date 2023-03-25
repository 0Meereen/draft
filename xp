    addXP(inter.user.id, inter.guild.id, 50);

    function calculateLevel(xp) {
      const LEVEL_THRESHOLD = 100; // Le nombre d'XP requis pour atteindre chaque niveau
      
      let level = 1;
      while (xp >= LEVEL_THRESHOLD) {
        level++;
        xp -= LEVEL_THRESHOLD;
      }
      return level;
    }
    

    function addXP(userId, guildId, xpToAdd) {
      try {
        const userDataDir = path.resolve(__dirname, `../../userdata/${guildId}`);
        if (!fs.existsSync(userDataDir)) {
          fs.mkdirSync(userDataDir);
        }
        const userFilePath = path.resolve(userDataDir, `${userId}.json`);
        if (!fs.existsSync(userFilePath)) {
          console.log("User data file does not exist!");
          return;
        }
    
        const userData = JSON.parse(fs.readFileSync(userFilePath));
        if (userData.guildId !== guildId) {
          console.log("User does not belong to this guild!");
          return;
        }
    
        const oldLevel = userData.level;
        userData.xp += xpToAdd;
    
        const newLevel = calculateLevel(userData.xp);
        if (newLevel > oldLevel) {
          // User has leveled up!
          userData.level = newLevel;
          userData.xp = 0; // Supprimer l'XP une fois que l'utilisateur atteint le niveau suivant
          fs.writeFileSync(userFilePath, JSON.stringify(userData, null, 2));
    
          // Send a message to the user
          const user = client.users.cache.get(userId);
          if (user) {
            inter.channel.send(`🎉 Congratulations ${inter.member}! You have reached level ${newLevel}! 🎉`);
          }
        } else {
          fs.writeFileSync(userFilePath, JSON.stringify(userData, null, 2));
        }
        
        console.log(`Added ${xpToAdd} XP to user ${userId}`);
      } catch (err) {
        console.error(err);
      }
    }
