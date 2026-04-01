javascript:(async function() { 
    async function getName(authToken) { 
        const response = await fetch('https://blooket.com+' + authToken); 
        const data = await response.json(); 
        return data.name; 
    }; 
    async function addCurrencies() { 
        const tokens = Number(prompt('How many tokens do you want to add? (500 daily max)')); 
        if (isNaN(tokens) || tokens <= 0) { 
            alert('Please enter a valid number.'); 
            return; 
        }
        const myToken = localStorage.token.split('JWT ')[1]; 
        if (tokens > 10000000) { 
            alert('Note: Blooket has a hard limit of 500 tokens daily. Amounts over this may not be added.'); 
        } 
        const response = await fetch('https://api.blooket.com/api/users/add-rewards', { 
            method: "PUT", 
            headers: { 
                "referer": "https://www.blooket.com/", 
                "content-type": "application/json", 
                "authorization": localStorage.token 
            }, 
            body: JSON.stringify({ 
                addedTokens: tokens, 
                addedXp: 300, 
                name: await getName(myToken) 
            }) 
        }); 
        if (response.status == 200) { 
            alert(`${tokens} tokens and 300 XP added to your account!`); 
        } else { 
            alert('An error occurred. Make sure you are logged into Blooket.'); 
        }; 
    }; 
    await addCurrencies(); 
})();
