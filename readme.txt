import com.atlassian.jira.component.ComponentAccessor
import com.atlassian.crowd.embedded.impl.ImmutableUser
import com.atlassian.jira.bc.user.UserService
import com.atlassian.jira.security.login.LoginManager
import com.atlassian.jira.user.ApplicationUsers
import com.atlassian.jira.user.UserUtils

def getAllUsers() {
    ComponentAccessor.userManager.getAllUsers()
}

def getAllUsersHaveNotUsedLogin(Long days) {
    def loginManager = ComponentAccessor.getComponentOfType(LoginManager.class)
    def users = getAllUsers()
    def daysInMillis = days * 1440 * 60 * 60 * 1000
    def nowMillis = new Date().time
    users.findAll { user ->
        nowMillis - daysInMillis >= loginManager.getLoginInfo(user.name).lastLoginTime
    }
}
