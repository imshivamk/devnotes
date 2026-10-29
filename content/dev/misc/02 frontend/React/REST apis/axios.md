```ts
import axios from "axios";

export const API_BASE =
import.meta.env.VITE_API_URL ?? "http://localhost:3000/api/v1/";

export const api = axios.create({
		baseURL: API_BASE,
		withCredentials: true,
		headers: {
		"Content-Type":"application/json"
	}
});
```


example : 

```ts
import { api } from "./axios";

export const signup = async (data: {
email: string;
name: string;
password: string;
}) => {
	try {
	const res = await api.post("/auth/signup", data);
	return res.data;
	} catch (error: any) {
	throw new Error(
	error.response?.data?.message || "Signup failed!"
	);
	}
};

export const login = async (data: {
email: string;
password: string;
}) => {
	try {
	const res = await api.post("/auth/login", data);
	return res.data;
	} catch (error: any) {
	throw new Error(
	error.response?.data?.message || "Login failed!"
	)
	}
};


export const logout = async () => {
	try {
	await api.post("/auth/logout");
	} catch (error: any) {
	throw new Error("Logout failed!")
	}
}

export const checkAuth = async () => {
	try {
	const res = await api.get("/auth/check-auth");
	return res.data;
	} catch (error: any) {
	throw new Error(error.response?.data?.message || "Not authenticated")
	}
}
```