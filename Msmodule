local MSmodule = {}

function MSmodule.rSample(n, range)
	-- n unique random ints from 1 to range
	if n > range then -- check for bad input
		return "u stupid"
	end
	
	math.randomseed(os.time())
	local out = {}
	for i=1, n do
		local try = math.random(1, range)
		while out[try] do
			try = math.random(1, range)
		end
		out[try] = try
	end
	return out
end


function MSmodule.zeros(shape)
	-- initializes an nD array of zeros of correct shape
	if #shape == 1 then
		local cols = shape[1]
		local tmp = {}
		for i=1, cols do
			tmp[i] = 0
		end
		return tmp
	
	else
		local tmp = {}
		local shapeTmp = {}
		for i=2, #shape do
			table.insert(shapeTmp, shape[i])
		end

		local outer = shape[1]
		for i=1, outer do
			table.insert(tmp, MSmodule.zeros(shapeTmp))
		end
		return tmp
	end
end

function MSmodule.flatToShapedIndices(flat, shape)
	-- returns array of mine indices in an array of shape shape, using
	-- their positions on a 1D array
	local arr = {}
	local subArr = {}
	local shapeArr = {}
	for i=1, #shape do
		local tmp = 1
		for j=i, #shape do
			if j >= #shape then
				tmp = tmp * 1
			else
				tmp = tmp * shape[j+1]
			end
		end
		shapeArr[i] = tmp
		tmp = 1
	end

	for _, v in pairs(flat) do
		local modVal = v
		for j, v2 in ipairs(shapeArr) do
			subArr[j] = math.ceil(modVal / v2)
			modVal = modVal % v2
			if modVal == 0 then
				modVal = v2
			end
		end
		table.insert(arr, subArr)
		subArr = {}
	end
	return arr
end

function MSmodule.put(a, ind, v, increment:boolean) -- numpy?? who???
	-- accepts ONE index of ONE mine/thing
	local subArr = a
	for j=1, (#ind - 1) do
		subArr = subArr[ind[j]]
		
	end
	if increment then
		subArr[ind[#ind]] += 1
	else
		subArr[ind[#ind]] = v
	end
	return a
end

function MSmodule.TableConcat(t1,t2)
	for i=1,#t2 do
		t1[#t1+1] = t2[i]
	end
	return t1
end

function MSmodule.indexNearbyTiles(inds, shape)
	-- returns the indices of all tiles around each mine
	local function nearby(idx, shape)
		-- gets indices of tiles around one mine
		if #idx == 1 then
			local dim = idx[1] -- highest dimension
			local transform = {-1, 0, 1}
			local min, max = 1, shape[1]
			local out = {}
			for _, v in transform do
				local tmp = dim + v
				if min <= tmp and tmp <= max then
					table.insert(out, {tmp})
				end
			end
			return out
			
		else
			local dim = idx[1]
			local transform = {-1, 0, 1}
			local min, max = 1, shape[1]
			local out = {}
			
			local subIdx = {}
			for i, v in ipairs(idx) do
				if i ~= 1 then
					table.insert(subIdx, v)				
				end
			end
			
			local subShape = {}
			for i, v in ipairs(shape) do
				if i ~= 1 then
					table.insert(subShape, v)				
				end
			end

			local subDims = nearby(subIdx, subShape)
			
			for _, v in transform do
				local tmp = dim + v
				if min <= tmp and tmp <= max then
					for _, subDim in pairs(subDims) do
						table.insert(out, MSmodule.TableConcat({tmp}, subDim))
				    end
				end
			end
			return out
		end
	end
	
	local out = {}
	for _, idx in pairs(inds) do
		table.insert(out, nearby(idx, shape))
	end
	return out
end


function MSmodule.printBoard(arr, out)
	-- prints a formatted board
	local out = out or ""
	if type(arr[1]) == 'number' then -- reached a flat array
		out = out .. (table.concat(arr, '\t'))
		return out

	else
		for _, v in ipairs(arr) do
			out = out .. ("\n" .. MSmodule.printBoard(v))
		end
	end
	return out
end


return MSmodule
