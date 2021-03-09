var tempErrorCreds;
var tempProviderName;


function firebaseLogin(providerName) {
	var user = firebase.auth().currentUser;
	if(!user){
		var provider = getProvider(providerName);
		firebase.auth().useDeviceLanguage();
		
		firebase.auth().signInWithPopup(provider).catch(function(error) {
			// Handle Errors here.
			var errorCode = error.code;
			var errorMessage = error.message;
			// The email of the user's account used.
			var email = error.email;
			// The firebase.auth.AuthCredential type that was used.
			tempErrorCreds = error.credential;
			console.log(error);
			
			if (errorCode === 'auth/account-exists-with-different-credential') {
				// User's email already exists.
				// Get sign-in methods for this email.
				firebase.auth().fetchSignInMethodsForEmail(email).then(function(methods) {
				  // the first method in the list will be the "recommended" method to use.
				  if(methods.length == 0)
					  return;
				  // Sign in to provider.
				  tempProviderName = methods[0].trim();
				  setModalContent("generalModalContent",
				  "<div id =\"continueWindow\"><span class=\"close\" id=\"closeButton\" onclick=\"hideModal('generalModal')\">&times;</span><p>Please press the button to login: </p><button onclick=\"continueLogin()\">Continue Login</button></div>");
				  showModal("generalModal");
				});
			}
		});
	}
	else
	{
		console.log("Already Logged in");
	}    
}

function firebaseLogout() {
	firebase.auth().signOut().catch(function(error) {
	  console.log(error);
	});   
}

function getCurrentUserDisplayName() {
	var user = firebase.auth().currentUser;
	var displayName = "";
	if(user)
	{
		displayName = user.displayName;
	}
	return displayName;
}

function getProvider(providerName){
	if(providerName && providerName.indexOf("facebook") != -1)
		return new firebase.auth.FacebookAuthProvider()
	else
		return new firebase.auth.GoogleAuthProvider()
}

function setModalContent(modalContentId, contentString){
	content = document.getElementById(modalContentId);
	if(content){
		content.innerHTML = contentString;
	}
		
}

function continueLogin(){
	hideModal("generalModal");
	var provider = getProvider(tempProviderName);
	firebase.auth().signInWithPopup(provider).then(
		function(result) {
			if(!tempErrorCreds){
				return;
			}
			// As we have access to the pending credential, we can directly call the link method.
			result.user.linkAndRetrieveDataWithCredential(tempErrorCreds).then(function(usercred) {
			  //goToApp();
			});
		});
	
}

function showModal(modalId){
	modal = document.getElementById(modalId);
	if(modal)
		modal.style.display = "block";
}

function hideModal(modalId){
	modal = document.getElementById(modalId);
	if(modal)
		modal.style.display = "none";
}
